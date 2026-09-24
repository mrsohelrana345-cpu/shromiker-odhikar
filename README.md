# শ্রমিকের অধিকার — বাস্তব সিস্টেমের Starter

## কী আছে
- শ্রমিক অভিযোগ ফর্ম
- Complaint ID
- SQLite database
- JPG/PNG/PDF evidence upload (5MB)
- অভিযোগ tracking
- JWT protected admin login
- Admin status update
- Basic security headers + rate limiting

## চালানো
1. Node.js 18+ ইনস্টল করুন।
2. Terminal খুলে project folder-এ যান।
3. `npm install`
4. Production-এর আগে environment variables দিন:
   - `JWT_SECRET` = দীর্ঘ random secret
   - `ADMIN_USER`
   - `ADMIN_PASSWORD`
5. `npm start`
6. Browser: `http://localhost:3000`

## গুরুত্বপূর্ণ
এটি production-ready legal case-management system নয়। Public launch-এর আগে:
- HTTPS
- strong admin password + MFA
- encrypted backups
- access logging
- secure file scanning
- privacy/consent policy
- data retention/deletion policy
- abuse/spam protection
- legal review
- authorized complaint-handling workflow
যোগ করুন।

প্রকাশ্যে evidence files serve করা হয়নি; এই starter-এ uploads private server directory-তে থাকে।
<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>শ্রমিকের অধিকার | অভিযোগ কেন্দ্র</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:"Noto Sans Bengali",Arial,sans-serif;
}

body{
    background:#f4f7f9;
    color:#222;
}

header{
    background:#0b5d3b;
    color:white;
    padding:16px 5%;
    display:flex;
    align-items:center;
    justify-content:space-between;
    flex-wrap:wrap;
}

.logo{
    font-size:24px;
    font-weight:bold;
}

.logo span{
    color:#ffd43b;
}

nav a{
    color:white;
    text-decoration:none;
    margin-left:18px;
    font-size:15px;
}

.hero{
    background:linear-gradient(135deg,#0b5d3b,#138a5b);
    color:white;
    padding:65px 20px;
    text-align:center;
}

.hero h1{
    font-size:38px;
    margin-bottom:15px;
}

.hero p{
    font-size:18px;
    max-width:700px;
    margin:auto;
    line-height:1.8;
}

.btn{
    display:inline-block;
    margin-top:25px;
    padding:13px 25px;
    background:#ffd43b;
    color:#222;
    text-decoration:none;
    border-radius:7px;
    font-weight:bold;
}

.container{
    width:90%;
    max-width:1000px;
    margin:40px auto;
}

.section-title{
    text-align:center;
    margin-bottom:25px;
    color:#0b5d3b;
}

.card{
    background:white;
    padding:28px;
    border-radius:12px;
    box-shadow:0 4px 18px rgba(0,0,0,.08);
    margin-bottom:30px;
}

.form-group{
    margin-bottom:18px;
}

label{
    display:block;
    margin-bottom:7px;
    font-weight:bold;
}

input,
select,
textarea{
    width:100%;
    padding:12px;
    border:1px solid #ddd;
    border-radius:7px;
    font-size:15px;
}

textarea{
    min-height:130px;
    resize:vertical;
}

button{
    width:100%;
    padding:14px;
    border:none;
    border-radius:7px;
    background:#0b5d3b;
    color:white;
    font-size:17px;
    cursor:pointer;
}

button:hover{
    background:#08492f;
}

.success{
    display:none;
    margin-top:20px;
    padding:18px;
    background:#e8fff2;
    border:1px solid #72c99b;
    border-radius:8px;
}

.complaint-id{
    font-size:22px;
    font-weight:bold;
    color:#0b5d3b;
}

.features{
    display:grid;
    grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
    gap:20px;
}

.feature{
    background:white;
    padding:25px;
    text-align:center;
    border-radius:10px;
    box-shadow:0 3px 12px rgba(0,0,0,.07);
}

.feature h3{
    margin:12px 0;
    color:#0b5d3b;
}

.track-result{
    display:none;
    margin-top:20px;
    padding:20px;
    background:#f0fff7;
    border-radius:8px;
}

footer{
    background:#12352a;
    color:white;
    text-align:center;
    padding:30px 15px;
    margin-top:50px;
    line-height:1.8;
}

@media(max-width:600px){

    header{
        text-align:center;
        justify-content:center;
        gap:12px;
    }

    nav{
        width:100%;
    }

    nav a{
        margin:0 7px;
    }

    .hero h1{
        font-size:29px;
    }

    .hero p{
        font-size:16px;
    }

    .card{
        padding:20px;
    }
}
</style>
</head>

<body>

<header>

    <div class="logo">
        শ্রমিকের <span>অধিকার</span>
    </div>

    <nav>
        <a href="#home">হোম</a>
        <a href="#complaint">অভিযোগ</a>
        <a href="#track">ট্র্যাক করুন</a>
        <a href="#help">সহায়তা</a>
    </nav>

</header>


<section class="hero" id="home">

    <h1>আপনার অধিকার, আপনার কণ্ঠ</h1>

    <p>
        বেতন না পাওয়া, অন্যায়ভাবে চাকরি থেকে বের করে দেওয়া,
        ওভারটাইমের টাকা না পাওয়া বা কর্মক্ষেত্রে অন্যায়ের
        বিষয়ে আপনার অভিযোগ জানান।
    </p>

    <a href="#complaint" class="btn">
        অভিযোগ করুন
    </a>

</section>


<section class="container">

    <h2 class="section-title">
        আমরা যেসব অভিযোগ গ্রহণ করি
    </h2>

    <div class="features">

        <div class="feature">
            <div style="font-size:40px;">💰</div>
            <h3>বেতন বকেয়া</h3>
            <p>বেতন না পাওয়া বা বেতন আটকে রাখার অভিযোগ।</p>
        </div>

        <div class="feature">
            <div style="font-size:40px;">📄</div>
            <h3>চাকরি থেকে বাদ</h3>
            <p>অন্যায়ভাবে চাকরি থেকে বের করে দেওয়ার অভিযোগ।</p>
        </div>

        <div class="feature">
            <div style="font-size:40px;">⏰</div>
            <h3>ওভারটাইম</h3>
            <p>ওভারটাইম কাজ করেও টাকা না পাওয়ার অভিযোগ।</p>
        </div>

        <div class="feature">
            <div style="font-size:40px;">⚖️</div>
            <h3>অন্যান্য সমস্যা</h3>
            <p>কর্মক্ষেত্রে অন্যান্য শ্রম অধিকার সংক্রান্ত অভিযোগ।</p>
        </div>

    </div>

</section>


<section class="container" id="complaint">

    <h2 class="section-title">
        অভিযোগ জমা দিন
    </h2>

    <div class="card">

        <form id="complaintForm">

            <div class="form-group">
                <label>আপনার নাম</label>
                <input type="text" id="name"
                       placeholder="আপনার নাম লিখুন">
            </div>

            <div class="form-group">
                <label>মোবাইল নম্বর *</label>
                <input type="tel" id="phone"
                       placeholder="01XXXXXXXXX"
                       required>
            </div>

            <div class="form-group">
                <label>গার্মেন্টস/কারখানার নাম *</label>
                <input type="text" id="factory"
                       placeholder="কারখানার নাম"
                       required>
            </div>

            <div class="form-group">
                <label>কারখানার ঠিকানা</label>
                <input type="text" id="address"
                       placeholder="ঠিকানা">
            </div>

            <div class="form-group">
                <label>অভিযোগের ধরন *</label>

                <select id="type" required>

                    <option value="">
                        নির্বাচন করুন
                    </option>

                    <option>
                        বেতন না পাওয়া
                    </option>

                    <option>
                        অন্যায়ভাবে চাকরি থেকে বাদ
                    </option>

                    <option>
                        ওভারটাইমের টাকা না পাওয়া
                    </option>

                    <option>
                        ছুটির টাকা সংক্রান্ত সমস্যা
                    </option>

                    <option>
                        অন্যান্য
                    </option>

                </select>

            </div>

            <div class="form-group">
                <label>অভিযোগের বিস্তারিত *</label>

                <textarea id="details"
                    placeholder="ঘটনাটি বিস্তারিত লিখুন..."
                    required></textarea>
            </div>

            <div class="form-group">

                <label>
                    প্রমাণ/ডকুমেন্ট
                </label>

                <input type="file"
                       id="document">

            </div>

            <div class="form-group">

                <label>
                    আপনার পরিচয় গোপন রাখতে চান?
                </label>

                <select id="anonymous">

                    <option value="না">
                        না
                    </option>

                    <option value="হ্যাঁ">
                        হ্যাঁ
                    </option>

                </select>

            </div>

            <button type="submit">
                অভিযোগ জমা দিন
            </button>

        </form>


        <div class="success" id="success">

            <h3>✅ অভিযোগ সফলভাবে জমা হয়েছে</h3>

            <p>
                আপনার অভিযোগ নম্বর:
            </p>

            <p class="complaint-id"
               id="complaintId">
            </p>

            <p style="margin-top:10px;">
                এই নম্বরটি সংরক্ষণ করুন।
                পরবর্তীতে অভিযোগের অবস্থা জানতে এটি ব্যবহার করুন।
            </p>

        </div>

    </div>

</section>


<section class="container" id="track">

    <h2 class="section-title">
        অভিযোগের অবস্থা দেখুন
    </h2>

    <div class="card">

        <div class="form-group">

            <label>
                অভিযোগ নম্বর
            </label>

            <input type="text"
                   id="trackId"
                   placeholder="যেমন: SR-123456">

        </div>

        <button onclick="trackComplaint()">
            অভিযোগ খুঁজুন
        </button>

        <div class="track-result"
             id="trackResult">

            <h3>অভিযোগ পাওয়া গেছে</h3>

            <p style="margin-top:10px;">
                স্ট্যাটাস:
                <strong>যাচাই চলছে</strong>
            </p>

        </div>

    </div>

</section>


<section class="container" id="help">

    <h2 class="section-title">
        জরুরি সহায়তা
    </h2>

    <div class="card">

        <p style="line-height:2;">

            আপনার অভিযোগের সঙ্গে প্রমাণ থাকলে তা সংরক্ষণ করুন।
            যেমন—বেতন স্লিপ, নিয়োগপত্র, উপস্থিতির তথ্য,
            ব্যাংক লেনদেনের প্রমাণ বা সংশ্লিষ্ট নথি।

            <br><br>

            <strong>
            গুরুত্বপূর্ণ:
            </strong>
            এই ওয়েবসাইটের মাধ্যমে অভিযোগ জমা দেওয়া মানেই
            অভিযোগটি সত্য প্রমাণিত হয়েছে এমন নয়।
            প্রতিটি অভিযোগ যাচাই করা প্রয়োজন।

        </p>

    </div>

</section>


<footer>

    <strong>শ্রমিকের অধিকার</strong>

    <br>

    শ্রমিকদের অভিযোগ ও অধিকার বিষয়ক তথ্য প্ল্যাটফর্ম

    <br><br>

    © 2026 শ্রমিকের অধিকার

</footer>


<script>

let currentComplaint = "";

document.getElementById("complaintForm")
.addEventListener("submit", function(e){

    e.preventDefault();

    let number =
        Math.floor(100000 + Math.random() * 900000);

    currentComplaint = "SR-" + number;

    document.getElementById("complaintId")
    .innerText = currentComplaint;

    document.getElementById("success")
    .style.display = "block";

    this.reset();

    window.location.hash = "complaint";

});


function trackComplaint(){

    let id =
        document.getElementById("trackId").value.trim();

    let result =
        document.getElementById("trackResult");

    if(id === currentComplaint && id !== ""){

        result.style.display = "block";

    }else{

        result.style.display = "block";

        result.innerHTML =
        "<h3>অভিযোগ পাওয়া যায়নি</h3>" +
        "<p style='margin-top:10px'>" +
        "সঠিক অভিযোগ নম্বর লিখুন।" +
        "</p>";

    }

}

</script>

</body>
</html>