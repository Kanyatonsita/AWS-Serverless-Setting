# Sätta upp nytt projekt med serverless

Installerat serverless på datorn:
```
npm install -g serverless
```
I terminalen:
- Navigera till mapp där du vill ha projektet och skriv:
```
serverless
Välj AWS - Node.js - Starter 
Välj namn för projektet ex. dog-api-serverless
Register: n
Deploy: n
```
- Navigera till mappen som skapats.
Öppna projektet i vsCode och skriva;
```
service: booking-api
frameworkVersion: '3'
useDotenv: true

provider:
  name: aws
  runtime: nodejs18.x
  profile: ${env:AWS_PROFILE}
  region: eu-north-1
  iam:
    role: ${env:AWS_IAM_ROLE}

plugins:
  - serverless-webpack

package:
  individually: true

functions:
  getRooms:
    handler: functions/getRooms/index.handler
    events: 
      - httpApi:
          path: '/rooms'
          method: GET
          
  postRooms:
    handler: functions/postRooms/index.handler
    events: 
      - httpApi:
          path: '/rooms'
          method: POST
```

Installera Serverless webpack till projekt:
```
npm init -y (för att skapa package.json)
npm install --save-dev serverless-webpack (installerar webpack)
```
I serverless.yml lägg till:
```
plugins:
  - serverless-webpack

package:
  individually: true
```
Lägg till ny fil i projekt:
- webpack.config.js det ska innehålla:
```
module.exports = {
    mode: 'development'
};

För att ladda upp till aws:
sls deploy
```

###Om man får:
```
Error:
AWS profile ”din”_profil doesn't seem to be configured

Skriv:
aws configure --profile din_profil

Hämta nycklar på aws IAM. Skapa nya vid behov.

Sedan:
AWS Access Key ID : key
AWS Secret Access Key [None]: secretKey
Default region name [None]: eu-north-1
Default output format [None]:

Följt av:
sls deploy
```

# Sätta upp .env i projektet:

Ändra i serverless.yml:

Lägg till:
    useDotenv: true
```
service: booking-api
frameworkVersion: '3'
useDotenv: true
```
Ändra provider så att det ser ut så här:
```
provider:
  name: aws
  runtime: nodejs18.x
  profile: ${env:AWS_PROFILE}
  region: eu-north-1
  iam:
    role: ${env:AWS_IAM_ROLE}
```
Skapa ny-fil i roten av projektet:
```
Döp den till .env
```
I .env-filen lägg tii:
```
AWS_PROFILE="din-profil"
AWS_IAM_ROLE="arn:aws:iam::xxxxxxxxxxxx:role/dinroll"
```
I .gitignore lägg till:
```
.env
```

# Sätta upp DynamoDB i serverless projekt
```yml
resources:
  Resources:
    roomsDb:
      Type: AWS::DynamoDB::Table
      Properties:
        TableName: rooms-db # Database table name
        AttributeDefinitions: 
          - AttributeName: id # database item id
            AttributeType: S # String
        KeySchema:
          - AttributeName: id # database key id
            KeyType: HASH
        BillingMode: PAY_PER_REQUEST
```
Deploy to AWS: `sls deploy`




    

