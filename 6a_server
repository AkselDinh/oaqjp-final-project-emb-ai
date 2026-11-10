import requests
from flask import Flask, request, render_template
from EmotionDetection import emotion_detector

app = Flask(__name__)

@app.route('/')
def render_index_page():
    return render_template("index.html")

# Post-mortem: hàm cũng là 1 biến (first-class citizen)
# nếu đặt tên hàm trùng với tên emotion_detector được import
# sẽ bị override mất (Name Shadowing)
# dẫn đến bên dưới gọi emotion = emotion_detector() lỗi
# do Python sẽ coi là đang gọi đệ quy hàm này
@app.route('/emotionDetector')
def analyze_emotion():
    text_to_analyze = request.args.get('textToAnalyze')
    emotion = emotion_detector(text_to_analyze)
    return (
        f"For the given statement, the system response is "
        f"'anger': {emotion['anger']}, "
        f"'disgust': {emotion['disgust']}, "
        f"'fear': {emotion['fear']}, "
        f"'joy': {emotion['joy']} and "
        f"'sadness': {emotion['sadness']}. "
        f"The dominant emotion is {emotion['dominant_emotion']}."
    )

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000, debug=True)