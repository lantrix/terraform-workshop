# Terraform Files

In this section we are going to prepare our infrastructure using terraform files!

Here's the files we can now edit in our repo:

**`variables.tf`**

In this file, we are going to configure any variables we would like to use in our infrastructure as code

**`config/dev.tfvars and prod.tfvars`**

In this file, we are going to provide different variable values based on whether its considered `dev` or `prod`

**`s3_bucket.tf`**

In this file, we are going to configure an s3 bucket and bucket policy that is enabled for basic website hosting

Here's what we're creating:

![s3 bucket for web hosting](../images/s3-web-hosting.png)

---

## `variables.tf`

This is file used to declare what variables we will have to work with.

Let's declare `bucket_name` as a variable:

```
variable "bucket_name" {
  type        = string
  description = "The name of the bucket - this must be unique"
}
```

---

## `config/dev.tfvars and prod.tfvars`

Let's now give our `bucket_name` variable a value!

```
bucket_name = "devopsgirls-terraform-[XXXX]-[dev/prod]"
```

Where `XXXX` appears, we need to fill in with some information

As bucket names must be unique, replace `XXXX` with a unique value like your name!

---

## `s3_bucket.tf`

Here's where we create and configure an s3 bucket and policy for website hosting. You'll see we're using [Terraform 'resource'](https://www.terraform.io/docs/language/resources/syntax.html) for this.

Let's try and avoid a simple copy paste and take a look at the official documentation to fill in the required information!

```
# S3 bucket for web hosting
resource "aws_s3_bucket" "web_hosting_bucket" {
  bucket = "XXXX"
  acl    = "XXXX"

  website {
    index_document = "XXXX"
    error_document = "XXXX"
  }
}

resource "aws_s3_bucket_policy" "web_hosting_policy" {
  bucket = aws_s3_bucket.web_hosting_bucket.id

  # Terraform's "jsonencode" function converts a
  # Terraform expression's result to valid JSON syntax.
  policy = jsonencode({
    Version = "2012-10-17"
    Id      = "webhosting-bucket-policy"
    Statement = [
      {
        Sid       = "XXXX"
        Effect    = "XXXX"
        Principal = "*"
        Action    = "XXXX"
        Resource = [
          XXXX,
          "XXXX/*"
        ]
      },
    ]
  })
}

```

Where `XXXX` appears, we need to fill in some information:

### s3 Bucket

- `bucket` - we will be using our `bucket_name` variable here - here is the documentation on [How To Use Variables](https://www.terraform.io/docs/language/values/variables.html#using-input-variable-values)
- `acl` - let's take a look at the docs, which value do you think is best for web hosting? [Terraform AWS Docs ACL](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket#acl)
- `index_document` - we've provided you with some basic website files in the folder [website_files](/website_files)
- `error_document` - we've provided you with some basic website files in the folder [website_files](/website_files)

---

### `Bucket Policy`

AWS documentation provides us with the bucket policy required for web hosting, so let's use this to fill in the fields with `XXXX`

- `Sid, Effect, Action`: [s3 bucket policy for web hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-bucket-policies.html) - find "Granting read-only permission to an anonymous user"
- `Resource` - you'll need the s3 bucket ARN, which can be done with Terraform - [use this example to work out how to add your ARN](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_policy#basic-usage)

## [NEXT SECTION - Terraform Commands 👉🏽](06-deploy-update-destroy.md)
