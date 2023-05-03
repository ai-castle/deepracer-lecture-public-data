# Automated Model Submission in CloudShell

```bash
# Set variables
ModelName="my-first-model"
LEADERBOARD_URL="https://us-east-1.console.aws.amazon.com/deepracer/home?region=us-east-1#league/arn%3Aaws%3Adeepracer%3A%3A%3Aleaderboard%2F02220ebb-d31b-4ee4-856e-091d0277e874"

# get ARN and decode
LEADERBOARD_ARN_ENCODED=$(echo $LEADERBOARD_URL | grep -oP '(?<=arn%3A).*$')
LEADERBOARD_ARN=$(echo $LEADERBOARD_ARN_ENCODED | sed 's/%\([0-9A-Fa-f][0-9A-Fa-f]\)/\\x\1/g' | xargs -0 printf "%b")
LEADERBOARD_ARN="arn:$LEADERBOARD_ARN"

# install
pip3 install deepracer-utils
python3 -m deepracer install-cli --force

# Loop
while true
do
  # Get model ARN
  MODEL_ARN=$(aws deepracer list-models --model-type "REINFORCEMENT_LEARNING" --region us-east-1 --query "Models[?ModelName=='$ModelName'].ModelArn | [0]" --output text)
  # Submit leaderboard
  if response=$(aws deepracer create-leaderboard-submission --model-arn "${MODEL_ARN}" --leaderboard-arn "${LEADERBOARD_ARN}" --region "${AWS_REGION}" --terms-accepted 2>&1); then
      echo "[$(date '+%Y-%m-%d %H:%M:%S')] Successfully submitted"
  else
      if echo "${response}" | grep -q "TooManyRequestsException"; then
        echo "[$(date '+%Y-%m-%d %H:%M:%S')] Under evaluation"
      else
        echo "[$(date '+%Y-%m-%d %H:%M:%S')] Error ${response}"
      fi
  fi
  sleep 60 # Execute at 60 seconds intervals.
done


```

- 브라우저 별 개발자도구 여는 방법 정리: [Link](https://www.computerhope.com/issues/ch002153.htm)

```javascript
function ConnectButton(){
    console.log("Connect pushed"); 
    document.querySelector('#terminal-app').click(); 
}
setInterval(ConnectButton, 1000);
```


<!-- # Set variables
ModelName="my-first-model"
LEADERBOARD_URL="https://us-east-1.console.aws.amazon.com/deepracer/home?region=us-east-1#league/arn%3Aaws%3Adeepracer%3A%3A%3Aleaderboard%2F02220ebb-d31b-4ee4-856e-091d0277e874"

# get ARN and decode
LEADERBOARD_ARN_ENCODED=$(echo $LEADERBOARD_URL | grep -oP '(?<=arn%3A).*$')
LEADERBOARD_ARN=$(echo $LEADERBOARD_ARN_ENCODED | sed 's/%\([0-9A-Fa-f][0-9A-Fa-f]\)/\\x\1/g' | xargs -0 printf "%b")
LEADERBOARD_ARN="arn:$LEADERBOARD_ARN"

# install
pip3 install deepracer-utils
python3 -m deepracer install-cli --force

# Loop
while true
do
  # Get model ARN
  MODEL_ARN=$(aws deepracer list-models --model-type "REINFORCEMENT_LEARNING" --region us-east-1 --query "Models[?ModelName=='$ModelName'].ModelArn | [0]" --output text)
  # Submit leaderboard
  response=$(aws deepracer create-leaderboard-submission --model-arn "${MODEL_ARN}" --leaderboard-arn "${LEADERBOARD_ARN}" --region "${AWS_REGION}" --terms-accepted 2>&1)
  if [ $? -eq 0 ]; then
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] Successfully submitted"
  else
    if echo "${response}" | grep -q "TooManyRequestsException"; then
      echo "[$(date '+%Y-%m-%d %H:%M:%S')] Under evaluation"
    else
      echo "[$(date '+%Y-%m-%d %H:%M:%S')] Error ${response}"
    fi
  fi
  sleep 60 # Execute at 60 seconds intervals.
done -->








