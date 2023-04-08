# yandex-cloud-tts
```
ffmpeg -i '01. Course Overview.mp4' -vn -acodec libopus -y audio_new.ogg

export FOLDER_ID=xxx
export CLOUD_ID=xxxx

SERVICE_ACCOUNT_ID=$(yc iam service-account create --name my-service-account --format json | yq .id)
yc resource-manager folder add-access-binding default --role ai.speechkit-stt.user --subject serviceAccount:${SERVICE_ACCOUNT_ID}
yc resource-manager folder add-access-binding default --role storage.viewer --subject serviceAccount:${SERVICE_ACCOUNT_ID}
yc iam service-account list

yc iam service-account --folder-id ${FOLDER_ID} list

yc iam key create --service-account-name my-service-account --output key.json --folder-id ${FOLDER_ID}

yc config profile create sa-profile

yc config set service-account-key key.json

yc config set cloud-id ${CLOUD_ID}
yc config set folder-id ${FOLDER_ID}

export IAM_TOKEN=$(yc iam create-token)
echo ${IAM_TOKEN}

OPERATIONS_ID=$(curl -X POST \
    -H "Authorization: Bearer ${IAM_TOKEN}" \
    -d "@body.json" \
    https://transcribe.api.cloud.yandex.net/speech/stt/v2/longRunningRecognize | yq .id)

curl -H "Authorization: Bearer ${IAM_TOKEN}" \
    https://operation.api.cloud.yandex.net/operations/${OPERATIONS_ID}

```
