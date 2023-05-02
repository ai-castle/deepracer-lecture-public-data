# Automated_Model_Submission in CloudShell

```bash
# Set variables
LEADERBOARD_ARN="arn:aws:deepracer:::leaderboard/02220ebb-d31b-4ee4-856e-091d0277e874"
ModelName="my-first-model"

# install
pip3 install deepracer-utils
python3 -m deepracer install-cli --force

while true
do
  # Get model ARN
  MODEL_ARN=$(aws deepracer list-models --model-type "REINFORCEMENT_LEARNING" --region us-east-1 --query "Models[?ModelName=='$ModelName'].ModelArn | [0]" --output text)
  # Submit leaderboard
  if response=$(aws deepracer create-leaderboard-submission --model-arn "${MODEL_ARN}" --leaderboard-arn "${LEADERBOARD_ARN}" --region "${AWS_REGION}" --terms-accepted 2>&1); then
      echo "[$(date '+%Y-%m-%d %H:%M:%S')] Successfully submitted !!"
  else
      if echo "${response}" | grep -q "TooManyRequestsException"; then
        echo "[$(date '+%Y-%m-%d %H:%M:%S')] Under evaluation"
      else
        echo "[$(date '+%Y-%m-%d %H:%M:%S')] Error ${response}"
      fi
  fi
  sleep 60 # 1분 간격으로 실행
done

```