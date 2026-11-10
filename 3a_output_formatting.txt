import json
import requests

def emotion_detector(text_to_analyze):

    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    headers = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    input_json = { "raw_document": { "text": text_to_analyze } }

    response = requests.post(url, json=input_json, headers=headers)
    data = json.loads(response.text)
    emotion = data['emotionPredictions'][0]['emotion']
    dominant_emotion = max(emotion, key=emotion.get)

    emotion['dominant_emotion'] = dominant_emotion
    return emotion