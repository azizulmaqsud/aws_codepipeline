# aws_codepipeline

version: 0.2

phases:

  pre_build:
  
    commands:
    
      - echo Logging into Amazon ECR...
      
      - aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login
      
        --username AWS --password-stdin
        
        $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com
        
      - REPOSITORY_URI=$AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME
      
      - COMMIT_HASH=$(echo $CODEBUILD_RESOLVED_SOURCE_VERSION | cut -c 1-7)
      
      - IMAGE_TAG=${COMMIT_HASH:=latest}
      
  build:
  
    commands:
    
      - echo Build started on `date`
      
      - echo Building the Docker image...
      
      - docker build -t $REPOSITORY_URI:latest .
      
      - docker tag $REPOSITORY_URI:latest $REPOSITORY_URI:$IMAGE_TAG
      
  post_build:
  
    commands:
    
      - echo Build completed on `date`
      
      - echo Pushing the Docker image...
      
      - docker push $REPOSITORY_URI:latest
      
      - docker push $REPOSITORY_URI:$IMAGE_TAG
      
      - echo Writing image definitions file...
      
      - printf '[{"name":"%s","imageUri":"%s"}]' "$IMAGE_REPO_NAME"
      
        "$REPOSITORY_URI:$IMAGE_TAG" > imagedefinitions.json
        
artifacts:

  files: imagedefinitions.json

## Thank You!
# Stay Connected !!

https://www.youtube.com/channel/UCNwP7KEElaJ7cdDTLP-KbBg

https://www.linkedin.com/in/azizul-maqsud/

https://azizulmaqsud-1684501031000.hashnode.dev/

https://medium.com/@azizulmaqsud

https://twitter.com/Sohail2me

https://github.com/azizulmaqsud


<a href="https://www.buymeacoffee.com/azizulmaqsud"><img src="https://img.buymeacoffee.com/button-api/?text=Buy me a coffee&emoji=&slug=scaleupsaas&button_colour=FFDD00&font_colour=000000&font_family=Cookie&outline_colour=000000&coffee_colour=ffffff" /></a>
