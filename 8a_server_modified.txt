"""
This module provide function to run a flask server
and calling to Watson NLP for Emotion Detection
"""

from flask import Flask, request, render_template
from EmotionDetection import emotion_detector

app = Flask(__name__)

@app.route('/')
def render_index_page():
    """
    Return the UI to display for user at Frontend
    """
    return render_template("index.html")

# Post-mortem: hàm cũng là 1 biến (first-class citizen)
# nếu đặt tên hàm trùng với tên emotion_detector được import
# sẽ bị override mất (Name Shadowing)
# dẫn đến bên dưới gọi emotion = emotion_detector() lỗi
# do Python sẽ coi là đang gọi đệ quy hàm này
@app.route('/emotionDetector')
def analyze_emotion():
    """
    Call to emotion_detection module to assess emotion from user
    """

    text_to_analyze = request.args.get('textToAnalyze')
    emotion = emotion_detector(text_to_analyze)

    if emotion['dominant_emotion'] is None:
        return "Invalid text! Please try again!."

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
