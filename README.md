### N8N Deployment on Ubuntu

N8n is installed with npm and pm2. The following steps will guide you through the troubleshooting process for a failed N8n deployment on Ubuntu.

### Install location of n8n
N8n is installed in the `/home/ubuntu/.`

PM2 starts a file called `ecosystem.config.js` which is located in the `/home/ubuntu` directory. This file contains the configuration for PM2 to manage the N8n process.

It also contains the variables for the N8n deployment, such as the port number and the database connection string. To add the environment variables, you can edit the `ecosystem.config.js` file and add the variables under the `env` section. For example:
```javascriptmodule.exports = {
  apps: [
    {
      name: 'n8n',
      script: 'n8n',
      env: {
        N8N_PORT: 5678,
        N8N_DB_TYPE: 'postgresdb',
        N8N_DB_POSTGRESDB_HOST: 'localhost',
        N8N_DB_POSTGRESDB_PORT: 5432,
        N8N_DB_POSTGRESDB_DATABASE: 'n8n',
        N8N_DB_POSTGRESDB_USER: 'n8n',
        N8N_DB_POSTGRESDB_PASSWORD: 'password',
      },
    },
  ],
};
```

### Check PM2 logs
Always run as root user when using PM2, as it may cause permission issues. 

```
sudo su
```


To check the PM2 logs for the N8n process, you can use the following command:
```bash
cd /home/ubuntu
pm2 logs n8n
```

Tail the logs to see the most recent entries:
```bash
pm2 logs n8n --lines 100
```

These command will display the logs for the N8n process, which can help you identify any errors or issues that may be causing the deployment to fail.

### Check N8n logs
N8n also generates its own logs, which can be found in the `/home/ubuntu/.n8n` directory. You can check the N8n logs by running the following command:
```bashcd /home/ubuntu/.n8n
ls -l /home/ubuntu/.n8n
```
This command will list the files in the `.n8n` directory, including the log files. You can view the contents of the log files using a text editor or by using the `cat` command:
```bash
cat /home/ubuntu/.n8n/n8nEventLog.log
```

### PM2 Commands
To start the N8n process using PM2, you can use the following command:
```bash
pm2 start ecosystem.config.js
```

To stop the N8n process, you can use the following command:
```bash
pm2 stop n8n
```

To restart the N8n process, you can use the following command:
```bash
pm2 restart n8n
```

To check the status of the N8n process, you can use the following command:
```bash
pm2 status n8n
```

### Conclusion
In most cases, `pm2 stop n8n`, `pm2 delete n8n`, and then `pm2 start ecosystem.config.js` will resolve the issue. If the issue persists, checking the PM2 logs and N8n logs can provide insights into what might be causing the deployment to fail. Always ensure that you are running PM2 as the root user to avoid permission issues.