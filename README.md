
# Chat Project

Built a chat application with Ruby on Rails that
* Allows users to sign up and sign in using `AWS Cognito` underlying backend
* Uses `Stimulus` and `Turbo` to more simply display html webpages
* Retains user messages from session to session
* User messages deletable by owner
* Stores messages in `AWS RDS`
* Uses `Elastic Beanstalk` to deploy
* Only deployed in `us-east-1`

## Running locally
These are the steps you can follow if you'd like to run the application locally, note there is a requirement that you set up your own aws account and set up RDS the same way.

1. Install Ruby for your appropriate CLI
 * If using Mac just `brew install rbenv` and `rbenv install 3.3.6`
1. Run XYZ command to set up project
2. Go into your AWS account and set up an access key for the admin role you use to sign in
 * This is not best practice but it is easy, otherwise the access key id and password you use in the next step needs to be generated every 12 hours
   * The correct way to do this is to run `aws sts assume-role --role-arn <ROLE_ARN_WITH_SAME_SCOPE_AS_REAL_APPLICATION> --duration-seconds 43200` then copy the outputted AccessKeyId and SecretAccessKey into the `aws configure` command passing the additional `--profile <any_profile_name_you_want>` parameter and then setting the `AWS_PROFILE` environment variable on your local system so the aws ruby sdk automatically assumes the profile you just set up
3. Run `aws configure` to authenticate to your aws account using the above access key id and secret key and set your default region to `us-east-1` or whichever region you intend to use
 * This assumes you have AWS CLI installed
  * If using Mac just `brew install awscli`
4. Set up an RDS cluster
 * Params:
   * Standard create
   * MySQL
   * Free Tier
   * Username: Admin
   * Credentials management
     * Self managed
     * Auto generate password: hGydXYdCAY5RJGqZ6LZR
   * Instance Configuration
     * Burstable classes
       * db.t3.micro
   * Connectivity
     * Use defaults
     * Public access
       * Yes
   * Database authentication
     * Password authentication
   * Additional Configuration
     * Initial database name
       * MessagesDB
   * Backup
     * No backups
   * Encryption
     * No encryption
 * Make sure security group allows access from your local ip on port 3306
 * Download SQL Workbench and set up table
   * Set user/pass/dns from what RDS gives you in the console
   * Going to need a user/pass, set those in the XYZ param in the app (make sure you never commit these in git)
   * ```
     CREATE TABLE `MessagesDB`.`messages_table` (
     `idmessages_table` INT NOT NULL AUTO_INCREMENT,
     `message` VARCHAR(100) NOT NULL,
     `userid` INT NOT NULL,
     PRIMARY KEY (`idmessages_table`),
     UNIQUE INDEX `idmessages_table_UNIQUE` (`idmessages_table` ASC) VISIBLE);
     ```
 * Set the url in the XYZ param in the app
5. Set up a Cognito user pool
 * Sign in options
   * User name
 * Security Requirements
   * Custom
     * Minimum length: 6
     * No requirements
     * No MFA
     * No self-service recovery
 * Message delivery
   * Send email with Cognito
   * Pool name: ChatApp
6. Run XYZ command to run the project

## Running the app in AWS
If you already followed all of the above steps you only have a couple more things to do to run the app in AWS and access it from the internet
1. Set up Elastic Beanstalk
 * assumes default vpc?
2. Create IAM role for Elastic Beanstalk instance
 * XYZ permissions
 * XYZ trust policy
3. (May not be necessary) Set up a DNS in Route53 that points at the Beanstalk DNS
 * Setup hosted zone, purchase domain, etc.
4. (May not be necessary) Set up a Certificate for the above DNS
5. (May not be necessary) Add the Certificate to the Elastic Beanstalk thing

