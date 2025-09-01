from flask import Flask, request, jsonify
from playwright.sync_api import sync_playwright

app = Flask(__name__)

@app.route("/")
def home():
    return "✅ TikTok Signing API is running!"

@app.route("/sign", methods=["POST"])
def sign():
    data = request.json
    url = data.get("url")

    if not url:
        return jsonify({"error": "URL is required"}), 400

    with sync_playwright() as p:
        browser = p.chromium.launch(headless=True, args=["--no-sandbox"])
        page = browser.new_page()

        # ندخل على TikTok
        page.goto(url)

        # هنا مثلا نرجع User-Agent كتوقيع (ممكن تعدله لأي شيء)
        headers = page.evaluate("() => navigator.userAgent")

        browser.close()

    return jsonify({
        "url": url,
        "headers": headers
    })
