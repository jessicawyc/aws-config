# aws-config
aws config related topics
improved from https://aws.amazon.com/cn/premiumsupport/knowledge-center/recreate-config-delivery-channel/
## 关闭config,恢复到初始状态
### 设置region参数 set parameter of regions you would like to run the command
```
regions=($(aws ec2 describe-regions --query 'Regions[*].RegionName' --output text))
```

### CLI命令 run the CLI command to totally disable config
```
for region in $regions; do
aws configservice stop-configuration-recorder --configuration-recorder-name $(aws configservice describe-configuration-recorders --region=$region --query 'ConfigurationRecorders[0].name' --output text) --region=$region
aws configservice delete-delivery-channel --delivery-channel-name $(aws configservice describe-delivery-channels --region=$region --query 'DeliveryChannels[0].name' --output text) --region=$region
aws configservice delete-configuration-recorder --configuration-recorder-name $(aws configservice describe-configuration-recorders --region=$region --query 'ConfigurationRecorders[0].name' --output text) --region=$region
echo $region
done
```
