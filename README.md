# sagemaker-setup
Useful scripts for making AWS SageMaker better

Guides:
- [SageMaker: auto stop your instances when idle](https://biasandvariance.com/sagemaker-stop-your-instances-when-idle/)
- [SageMaker: setup SSH](https://biasandvariance.com/sagemaker-ssh-setup/)
- [SageMaker: save your conda environments after the machine restart](https://biasandvariance.com/sagemaker-save-your-conda-environments/)

Scripts:
- auto stop your instances when inactive!
Example usage:
```bash
#!/bin/bash
set -e

# PARAMETERS
IDLE_TIME=10800
COMMIT_SHA="22c742dae52ee043f3af0d26d2ca5ad1f21b8c21"

echo "Fetching the autostop script"
wget -O autostop.py https://raw.githubusercontent.com/Tonoward/sagemaker-setup/${COMMIT_SHA}/scripts/auto-stop-idle/autostop.py

echo "Starting the SageMaker autostop script in cron"
(crontab -l 2>/dev/null; echo "*/5 * * * * /bin/bash -c '/usr/bin/python3 $DIR/autostop.py --time ${IDLE_TIME} | tee -a /home/ec2-user/SageMaker/auto-stop-idle.log'") | crontab -

echo "Changing cloudwatch configuration"
curl https://raw.githubusercontent.com/Tonoward/sagemaker-setup/${COMMIT_SHA}/scripts/publish-logs-to-cloudwatch/on-start.sh | sudo bash -s auto-stop-idle /home/ec2-user/SageMaker/auto-stop-idle.log

```


- send additional log files to CloudWatch
- install nb/server/lab extensions
- setup ssh
