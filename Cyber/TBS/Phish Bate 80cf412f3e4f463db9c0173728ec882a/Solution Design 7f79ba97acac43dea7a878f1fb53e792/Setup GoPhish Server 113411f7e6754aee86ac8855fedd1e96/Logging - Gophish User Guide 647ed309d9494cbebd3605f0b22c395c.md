# Logging - Gophish User Guide

[https://docs.getgophish.com/user-guide/documentation/logging](https://docs.getgophish.com/user-guide/documentation/logging)

![-LOA_pe0J_YIk3ySgPqb.png](Logging%20-%20Gophish%20User%20Guide%20647ed309d9494cbebd3605f0b22c395c/-LOA_pe0J_YIk3ySgPqb.png)

By default, logs are sent to the `stderr` filehandle in the terminal. However, there may be times you wish to store logs in a file.

### Sending Logs to a File

Prior to Gophish version 0.8.0, you can redirect logs from the terminal into a file using standard shell redirection:

$ ./gophish > gophish.log 2>&1

The downside to this is that logs will no longer show up in the terminal. Starting with Gophish version 0.8.0, you will have the option to configure additional logging directly within Gophish.

In your `config.json` file, modify the `logging` section to include whichever filename you wish to use for logging: