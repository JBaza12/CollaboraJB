---
layout: post
type: hacks
permalink: /postlogin
---
 <span style="font-size:4em;">Welcome to Collabora</span>

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Search Bar Example</title>
<style>

    #search-container {
        margin-bottom: 20px;
    }

    #search-input {
        width: 300px;
        padding: 8px;
        font-size: 16px;
    }

    #search-results {
        list-style-type: none;
        padding: 0;
    }

    #search-results li {
        margin-bottom: 5px;
    }
</style>
</head>
<body>

<div id="search-container">
    <input type="text" id="search-input" placeholder="Search a topic">
    <ul id="search-results"></ul>
    <button id="my-button" style="background-color: #c5000c; color: white;">Search</button>

</div>



<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Question Details</title>
    <style>
        .question-box {
            border: 1px solid #ccc;
            padding: 40px; /* Increase padding to make the box bigger */
            margin-bottom: 20px;
            border-radius: 5px;
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #fff;
            z-index: 1000;
            width: 80%; /* Set width to 80% of the viewport */
            max-width: 600px; /* Set maximum width to 600px */
        }
        .user-info {
            font-weight: bold;
            margin-bottom: 20px; /* Increase margin between user info and date */
        }
        .date {
            color: #666;
            font-size: 14px;
        }
    </style>
</head>
<body>

<div class="question-box">
    <div class="user-info">
        <p><label for="uid">User ID:< document.getElementById("uid").value </label> 
    </p>
        <span>Username: </span><span id="username">johndoe123</span>
    </div>
    <div class="date" id="post-date">Posted on: January 1, 2024</div>
</div>

</body>
</html>



<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Question Details</title>
    <style>
        .question-box {
            border: 1px solid #ccc;
            padding: 20px;
            margin-bottom: 20px;
            border-radius: 5px;
        }
        .user-info {
            font-weight: bold;
            margin-bottom: 10px;
        }
        .date {
            color: #666;
            font-size: 14px;
        }
        #search-container {
            margin-bottom: 20px;
            border: 1px solid #ccc;
             padding: 20px;
        }
        #ask-question-box {
            border: 1px solid #ccc;
            padding: 20px;
            border-radius: 5px;
        }
        #ask-question-box {
            border: 1px solid #ccc;
            padding: 20px;
            border-radius: 5px;
        }
        #ask-question-input {
            width: 20%; /* Set input width to 100% */
            height: 100px; /* Set input height to 100 pixels */
            padding: 8px;
            margin-top: 10px;
            box-sizing: border-box;
        }
    </style>
<script>
function getTodaysDate() {
    const today = new Date();
    const yyyy = today.getFullYear();
    let mm = today.getMonth() + 1; // Months start at 0!
    let dd = today.getDate();

    if (dd < 10) dd = '0' + dd;
    if (mm < 10) mm = '0' + mm;

    const formattedToday = yyyy+ "-" + mm + '-'+ dd;

    return formattedToday;
}

function createPost() {
    const enteredPost = document.getElementById("ask-question-input").value;
    const enteredDOQ = getTodaysDate()
    const enteredUid = "toby" //to be set dynamically (figure out later)
    const enteredId = "10" //to be set dynamically (figure out later)
    post_api(enteredId, enteredPost, enteredUid, enteredDOQ)

}

function post_api(id, post, uid, doq) {
    var myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");

    var raw = JSON.stringify({
    "id": id,
    "note": post,
    "uid": uid,
    "doq": doq
    });

    var requestOptions = {
    method: 'POST',
    headers: myHeaders,
    body: raw,
    redirect: 'follow'
    };

    fetch("http://127.0.0.1:8091/api/post/", requestOptions)
        .then(response => {
            if (response.ok) {
                console.log("Question Received");
                alert("Question has been sent, you will receive a response soon.");
              } else {
                console.error("Question creation failed");
                // You can handle failed login attempts here
                const errorMessageDiv = document.getElementById('errorMessage');
                errorMessageDiv.innerHTML = '<label style="color: red;">Question Creation Failed</label>';
              }
          })
          .then(result => { 
            console.log(result);
            
            })
          .catch(error => console.log('error', error)); 
   
}

  
</script>
</head>
<body>



<div class="question-box">
    <div class="user-info">
        <span>Name: </span><span id="user-name">John Doe</span><br>
        <span>Username: </span><span id="username">johndoe123</span>
    </div>
    <div class="date" id="post-date">Posted on: January 1, 2024</div>
</div>
<div id="errorMessage"></div>
<form action="javascript:createPost()">
<div id="ask-question-box">
    <span>Want to ask a question?</span><br>
    
    <input type="text" id="ask-question-input" placeholder="Enter your question here">
</div>
<button>           </button> 
<button id="my-button" style="background-color: #c5000c; color: white;">Submit</button>
</form>
</body>
</html>

