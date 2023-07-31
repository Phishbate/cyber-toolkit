# Handling Database Migrations in Go · Gophish - Blog

[https://getgophish.com/blog/post/database-migrations-in-go/](https://getgophish.com/blog/post/database-migrations-in-go/)

> 
> 
> 
> “I got my database schema correct on the first try.”
> 
> - No one ever.

Like most big projects, gophish needed a way to automatically manage changes to our database schema. As new features were being added, we found ourselves in a situation that required us to add or modify columns and tables to store the new data.

In a hosted environment, this is no problem since we control the database and can make schema changes as we see fit. Gophish is different, in that it is software intentionally designed to run on the client’s machine. This means that as we rollout updates to gophish’s backend database, we need a way to easily update (or rollback!) changes to the database structure. A versioning system is a perfect fit, which introduces the idea of migrations.

### What is a *Migration*?

A migration is nothing more than a set of SQL commands to make changes to a database. Every migration typically has two parts: how to apply the changes you want, and how to roll them back.

To version control our database, we can create a folder holding multiple migration files. Each file will have an identifier so we know which migration should be applied and in which order. Then, we can store which version our database is currently at in the database itself so if we ever add migrations in the future, we can tell where we left off.

There are tools that can automate this process for us. We settled on a well-known database migration tool called `[goose](https://bitbucket.org/liamstask/goose/)`.

### Introduction to `goose`

We chose to go with `[goose](https://bitbucket.org/liamstask/goose/)` since it seemed like a mature, fully-featured solution that would be easily integrated into our code. Goose typically works through the use of its command line tool aptly named `goose`.

To set things up, we first need to create the following folder structure:

```
| db/
| | migrations/
| `-dbconf.yml

```

Our migrations will be stored in the `migrations` folder as a series of SQL files. Before we can create migrations, we have to specify the configuration for `goose` to use. This is found in the `dbconf.yml` file. In our case, we used the following configuration:

```
production:
    driver: sqlite3
    open: gophish.db
    dialect: sqlite3
    import: github.com/mattn/go-sqlite3

```

This configuration specifies a single environment, `production`, that manages a SQLite database.

Now that we have created our configuration file, we can start making our migrations. Unfortunately, this is where the hurdles began.

### A Little About Gophish

Normally, migrations are something that is considered early on in the database creation process. Unfortunately, our schema was already defined and we had clients already running gophish. So, we needed to orchestrate `goose` in such a way that we could create and apply our migrations without messing up any data that was already in the client’s databases.

The first step was creating the migrations. To handle this, we first created an empty migration file using the following:

```
goose -env production create 0.1.2_browser_post sql
goose: created ~\go\src\github.com\gophish\gophish\db\migrations\20160130184410_0.1.2_browser_post.sql

```

This command created a new empty SQL file in our migrations folder that looks like this:

```
-- +goose Up
-- SQL in section 'Up' is executed when this migration is applied

-- +goose Down
-- SQL section 'Down' is executed when this migration is rolled back

```

For our first migration, we decided to baseline our schema to the current version. To do this, we simply exported our existing schema using the sqlite3 tool. That gave us all of our `CREATE TABLE` statements that setup our tables and default data. We then copy/pasted those statements below the `-- +goose Up` section of the migrations.

The one change we made was to add `IF NOT EXISTS` to all of our table creation statements. This meant that if the client already had a database setup, this migration would be applied, but no changes would be made - exactly what we want.

The final step to create this migration was to add the rollback statements. Since this was creating the database, `DROP TABLE` equivalent statements worked just fine. You can see our final migration file [here](https://raw.githubusercontent.com/gophish/gophish/master/db/migrations/20160118194630_init.sql).

Now for the next hurdle. Traditionally, migrations work by creating a new migration file and running `goose up`. Then, `goose` will compare your database version with the migration files it finds. If there are migrations that need to be applied, it will apply them in order until you are at the current version.

While the `goose up` command can work if we control the database, there’s simply no way that we can expect our users to install `goose` and run `goose up` every time we want to make a database change. Our goal has always been to make the lives of our users easier, so this simply wouldn’t work. This meant that we needed to handle the migrations in our code.

Fortunately for us, the `goose` CLI wraps a rich library that we can use. We were able to integrate this directly into our `Setup()` function to apply migrations automatically.

First, we created the `gooose.DBConf` struct to hold the configuration (a programmatic copy of our `dbconf.yml` file).

```
// Setup the goose configuration
migrateConf := &goose.DBConf{
	MigrationsDir: config.Conf.MigrationsPath,
	Env:           "production",
	Driver: goose.DBDriver{
		Name:    "sqlite3",
		OpenStr: config.Conf.DBPath,
		Import:  "github.com/mattn/go-sqlite3",
		Dialect: &goose.Sqlite3Dialect{},
	},
}
```

Next, we need to figure out the latest database version supported by our migrations. This gives us the final “goal” migration that we want to upgrade to. We can do this via the function `[goose.GetMostRecentDBVersion](https://godoc.org/bitbucket.org/liamstask/goose/lib/goose#GetMostRecentDBVersion)`.

```
// Get the latest possible migration
latest, err := goose.GetMostRecentDBVersion(migrateConf.MigrationsDir)
if err != nil {
	Logger.Println(err)
	return err
}
```

And finally, we need to apply our migrations. `Goose` has a function called `[goose.RunMigrationsOnDb](https://godoc.org/bitbucket.org/liamstask/goose/lib/goose#RunMigrationsOnDb)` which expects an existing `[sql.DB](https://golang.org/pkg/database/sql/#DB)` object. Since gophish uses the ORM `[gorm](https://github.com/jinzhu/gorm)`, we already had a `sql.DB` object already initialized that we could use to send to `goose`. This was stored in the `db` variable.

```
// Migrate up to the latest version
err = goose.RunMigrationsOnDb(migrateConf, migrateConf.MigrationsDir, latest, db.DB())
if err != nil {
	Logger.Println(err)
	return err
}
```

That’s it! You can find our full `Setup()` function [here.](https://github.com/gophish/gophish/blob/master/models/models.go#L61) To handle any additional migrations, all we need to do is run `goose create` again, add the SQL that makes up the migration, and push out the new file. The next time clients update gophish and restart the executable, the database migrations will be applied automatically!

If this kind of stuff is interesting to you, and you want to see a full example of a web app written in Go, check out gophish by clicking below.

[Download gophish](https://github.com/gophish/gophish)