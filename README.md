# Terraform Connector

## Configuration

This configuration process is a work in progress. This deploy process has not been widely used yet, so it may require troubleshooting.

- Create `terraform/terraform.tfvars`. Write the following configuration (replacing the names and paths with your actual SSH key name and public key path):

```
key_name="myKeyName"
key_path="/home/myUser/.ssh/myKeyFile.pub"
```

- Set the instances of `<YOUR_HOST>` in `terraform/main.tf` to the actual domain name you want to run your connector on.
- Set `<YOUR_RIPPLE_HOT_WALLET_SECRET>` in `salt/connector/files/launch.config.js` to your ripple hot wallet secret.
- Set `<GENERATE_SECRET>` to a securely generated **RANDOM** secret key, of at least 16 bytes. It MUST be URL-safe. For instance, `require('crypto').randomBytes(16).toString('hex')`.
- Set `<YOUR_PARENT_HOST>` to the BTP host of your parent connector. You can find a parent connnector on [Connector Land](https://connector.land) (WIP).

Now you're ready to deploy. If anything goes wrong, you can taint any components that encountered errors, and re-run `terraform apply` with your new configuration.

## Deploy

``` sh
export AWS_ACCESS_KEY=<YOUR_AWS_ACCESS_KEY>
export AWS_SECRET_KEY=<YOUR_AWS_SECRET_KEY>
cd terraform
terraform apply
```

## Re-deploy with latest provisioning data

``` sh
export AWS_ACCESS_KEY=<YOUR_AWS_ACCESS_KEY>
export AWS_SECRET_KEY=<YOUR_AWS_SECRET_KEY>
cd terraform
terraform taint aws_instance.web
terraform apply
```

## Test Salt states locally

``` sh
cd salt-test
docker-compose up
```
