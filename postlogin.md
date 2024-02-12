---
layout: post
type: hacks
permalink: /postlogin
---
<span style="font-size:4em;">Successfully logged in</span>

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
</head>
<body>



<div class="question-box">
    <div class="user-info">
        <span>Name: </span><span id="user-name">John Doe</span><br>
        <span>Username: </span><span id="username">johndoe123</span>
    </div>
    <div class="date" id="post-date">Posted on: January 1, 2024</div>
</div>

<div id="ask-question-box">
    <span>Want to ask a question?</span><br>
    <input type="text" id="ask-question-input" placeholder="Enter your question here">
</div>
<button>           </button> 
<button id="my-button" style="background-color: #c5000c; color: white;">Submit</button>

</body>
</html>