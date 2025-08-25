# Smart_arch_Ai
:->$$
from flask import Flask, request, render_template_string
import threading
import requests
from bs4 import BeautifulSoup
import ssl
import socket
import time

app = Flask(__name__)

# ----------------- بخش وب سایت -----------------
@app.route("/", methods=["GET", "POST"])
def home():
    message_sent = False
    if request.method == "POST":
        name = request.form.get("name")
        email = request.form.get("email")
        phone = request.form.get("phone")
        message = request.form.get("message")
        print(f"پیام جدید: {name}, {email}, {phone}, {message}")
        message_sent = True

    return render_template_string(r"""
<!DOCTYPE html>
<html lang="fa">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Smart Arch_AI </title>
<style>
body { font-family: Tahoma, sans-serif; background:#f4f4f4; margin:0; padding:0; }
header { background:#333; color:#fff; padding:20px; text-align:center; }
header h1 { font-size:2rem; }
nav { display:flex; justify-content:center; background:#444; padding:10px; }
nav a { color:#fff; margin:0 15px; text-decoration:none; }
nav a:hover { text-decoration:underline; }
.container { display:flex; flex-direction:column; align-items:center; padding:20px; }
.content { width:80%; max-width:1200px; margin-bottom:40px; padding:15px; background:rgba(255,255,255,0.7); border-radius:10px; }
.content h2 { font-size:1.8rem; margin-bottom:10px; }
.content p, .content li { font-size:1.1rem; margin-bottom:10px; }
ul { list-style-type: disc; margin-left:20px; }
.slideshow { width:100%; max-width:1200px; margin-bottom:40px; position:relative; overflow:hidden; border-radius:10px; }
.slideshow img { width:100%; height:auto; display:none; border-radius:10px; }
.slideshow img.active { display:block; }
.box { width:200px; padding:10px; background:rgba(52,152,219,0.8); color:#fff; text-align:center; border-radius:10px; cursor:pointer; margin:20px auto 5px; transition:0.3s; }
.box:hover { transform:translateY(-3px); background:rgba(41,128,185,0.9); }
.box.active { background:rgba(231,76,60,0.9) !important; }
.contact-form { width:100%; max-width:500px; background:#fff; padding:20px; border-radius:8px; box-shadow:0 4px 8px rgba(0,0,0,0.1); display:flex; flex-direction:column; gap:10px; }
.contact-form h3 { font-size:1.5rem; margin-bottom:10px; }
.contact-form input, .contact-form textarea { padding:10px; border-radius:6px; border:1px solid #ccc; font-size:1rem; width:100%; }
.contact-form button { padding:10px; background:#333; color:#fff; border:none; border-radius:6px; cursor:pointer; font-size:1.1rem; }
.contact-form button:hover { background:#555; }
.success-msg { text-align:center; color:green; font-weight:bold; }
footer { background:#333; color:#fff; text-align:center; padding:20px; }

/* لینک اینستاگرام */
.instagram-link {
    display:inline-block;
    margin-bottom:20px;
    color:#E1306C;
    font-weight:bold;
    font-size:18px;
    text-decoration:none;
}
.instagram-link:hover { text-decoration:underline; }
</style>
</head>
<body>

<header><h1>Smart Arch_AI </h1></header>

<nav>
<a href="#about">درباره من</a>
<a href="#skills">مهارت‌ها</a>
<a href="#portfolio">نمونه کارها</a>
<a href="#contact">تماس با من</a>
</nav>

<div class="container">

<!-- لینک اینستاگرام -->
<a href="https://www.instagram.com/arshiasdiarch/" target="_blank" class="instagram-link">
    من را در اینستاگرام دنبال کنید
</a>

<div class="box active" onclick="activateBox(this,'about')">درباره من</div>
<section id="about" class="content">
<p>عرشیا، دانشجوی مقطع کارشناسی در رشته‌ی معماری حرفه‌ای هستم. دلگرو هنر و دانش معماری اصیل ایرانی دارم و با شور و شوقی وصف‌ناشدنی به رازها و زیبایی‌های این میراث کهن می‌نگرم. در کنار این عشق به معماری، علاقه‌ای ژرف به دنیای هوش مصنوعی دارم و تلاش می‌کنم تا دانش نوین را با خرد و زیبایی معماری ایرانی پیوند زنم.</p>
</section>

<div class="box" onclick="activateBox(this,'skills')">مهارت‌ها</div>
<section id="skills" class="content">
<p>مهارت‌های نرم‌افزار:</p>
<ul>
<li>AutoCAD</li>
<li>Revit</li>
<li>Ai</li>
<li>html,css</li>
</ul>
</section>

<div class="box" onclick="activateBox(this,'portfolio')">نمونه‌ها</div>
<section id="portfolio" class="content">
<div class="slideshow">
<img src="https://images.unsplash.com/photo-1600585154340-be6161a56a0c" alt="Villa 1" class="active">
<img src="https://images.unsplash.com/photo-1542314831-068cd1dbfeeb" alt="Villa 2">
<img src="https://images.unsplash.com/photo-1553853830-6750b6b55669" alt="Villa 3">
<img src="https://images.unsplash.com/photo-1508057198894-247b23fe5ade" alt="Villa 4">
<img src="https://images.unsplash.com/photo-1494526585095-c41746248156" alt="Villa 5">
</div>
</section>

<section id="contact" class="content">
{% if message_sent %}
<div class="success-msg">درخواست شما ارسال شد!</div>
{% endif %}
<form method="post" class="contact-form">
<h3>فرم تماس</h3>
<input type="text" name="name" placeholder="نام کامل" required>
<input type="email" name="email" placeholder="example@email.com" required>
<input type="tel" name="phone" placeholder="+98******** همراه" required>
<textarea name="message" rows="4" placeholder="متن پیام" required></textarea>
<button type="submit">ارسال</button>
</form>
</section>

</div>

<footer>
<p>&copy; 2025  | تمامی حقوق محفوظ است</p>
</footer>

<script>
// هایلایت باکس و اسکرول
function activateBox(box, sectionId){
    let boxes = document.querySelectorAll('.box');
    boxes.forEach(b=>b.classList.remove('active'));
    box.classList.add('active');
    document.getElementById(sectionId).scrollIntoView({behavior:'smooth'});
}

// اسلایدشو نمونه‌ها
let slides = document.querySelectorAll('.slideshow img');
let currentSlide = 0;
setInterval(()=>{
    slides[currentSlide].classList.remove('active');
    currentSlide = (currentSlide+1)%slides.length;
    slides[currentSlide].classList.add('active');
},6000);
</script>

</body>
</html>
""", message_sent=message_sent)

# ----------------- ربات بررسی سایت -----------------
def check_site():
    url = "http://127.0.0.1:5000"
    try:
        response = requests.get(url, timeout=10)
        status = response.status_code
        html = response.text
        soup = BeautifulSoup(html, "html.parser")
        title = soup.title.string if soup.title else "بدون عنوان"

        if url.startswith("https://"):
            hostname = url.replace("https://","").split("/")[0]
            context = ssl.create_default_context()
            with socket.create_connection((hostname, 443)) as sock:
                with context.wrap_socket(sock, server_hostname=hostname) as ssock:
                    cert = ssock.getpeercert()
            ssl_status = "SSL معتبر است"
        else:
            ssl_status = "HTTP بدون SSL"

        print(f"[ربات] وضعیت سایت: {status}, عنوان: {title}, {ssl_status}")
    except Exception as e:
        print(f"[ربات] خطا در بررسی سایت: {e}")

def run_schedule():
    while True:
        check_site()
        time.sleep(8*60*60)  # هر 8 ساعت یکبار

# ----------------- اجرای همزمان Flask و ربات -----------------
if __name__=="__main__":
    import threading
    t = threading.Thread(target=run_schedule, daemon=True)
    t.start()
    app.run(debug=False)
