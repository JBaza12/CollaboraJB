---
layout: post
type: hacks
permalink: /question
---
 <span style="font-size:4em;">Welcome to Collabora</span>


<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Question Page</title>
   
    <style>
    .question-container, #answer-question-box {
        margin: 20px;
        padding: 20px;
        border: 1px solid #ccc;
        border-radius: 5px;
    }
    #answer-question-input {
        width: 100%; /* Adjusted to 100% for consistency */
        height: 100px;
        padding: 8px;
        margin-top: 10px;
        box-sizing: border-box;
    }
    #comments-container {
        margin: 20px;
        padding: 20px;
    }
    .comment {
        background-color: #f0f8ff; /* Soft Alice Blue */
        border-left: 5px solid #6495ed; /* Cornflower Blue for contrast */
        padding: 10px;
        margin-bottom: 10px;
        border-radius: 5px;
    }
    .comment p {
        margin: 0;
        color: #333; /* Darker text for readability */
    }
    .comment small {
        display: block;
        margin-top: 10px;
        color: #666; /* Lighter text for metadata */
    }
    #question-container {
        font-size: 1.5em; /* Increase the size */
        font-weight: bold; /* Make the text bold */
    }
</style>

</head>
<body>

<div id="question-container" class="question-container">
<h3>Question:</h3>
    <!-- Question will be inserted here -->
</div>
<div id="comments-container">
    <h3>Comments:</h3>
    <!-- Comments will be inserted here -->
</div>
<script>
    function reply_api(){
        var myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");

        const questionId = new URLSearchParams(window.location.search).get('questionId');
        const replyText = document.getElementById('answer-question-input').value;

        var raw = JSON.stringify({
            "parentPostId": questionId,
            "note": replyText,
            "uid": "toby"
            });

        var requestOptions = {
            method: 'POST',
            headers: myHeaders,
            body: raw,
            redirect: 'follow'
            };
              fetch("http://127.0.0.1:8086/api/post/", requestOptions)
                .then(response => {
                    if (response.ok) {
                        console.log("Reply Received");
                        alert("Your reply has been received.");
                        window.location.reload();

                    } else {
                        console.error("Reply failed");
                        // You can handle failed login attempts here
                        const errorMessageDiv = document.getElementById('errorMessage');
                        errorMessageDiv.innerHTML = '<label style="color: red;">Reply Failed</label>';
                    }
                })
    }
    
    async function fetchQuestion(questionId) {
        try {
            const response = await fetch(`http://127.0.0.1:8086/api/post?id=${questionId}`);
            if (!response.ok) {
                throw new Error(`Error: ${response.statusText}`);
            }
            const data = await response.json();
            return data.note; // Assuming the API returns an object with a 'note' property
        } catch (error) {
            console.error('Fetching question failed:', error);
            return "Question not found or there was an error fetching the question.";
        }
    }

    async function fetchAndDisplayComments(questionId) {
        const commentsContainer = document.getElementById('comments-container');
        try {
            const response = await fetch(`http://127.0.0.1:8086/api/post?parentPostId=${questionId}`);
            if (!response.ok) {
                throw new Error(`Error: ${response.statusText}`);
            }
            const comments = await response.json();
            if (comments.length > 0) {
                const commentsHTML = comments.map(comment => `
                    <div class="comment">
                        <p>${comment.note}</p>
                        <small>By: ${comment.userID}</small>
                    </div>
                `).join('');
                commentsContainer.innerHTML += commentsHTML;
            } else {
                commentsContainer.innerHTML += '<p>No comments found.</p>';
            }
        } catch (error) {
            console.error('Fetching comments failed:', error);
            commentsContainer.innerHTML += '<p>Error fetching comments.</p>';
        }
    }

    async function displayQuestion() {
        // Extracting questionId from URL parameters
        const urlParams = new URLSearchParams(window.location.search);
        const questionId = urlParams.get('questionId');
        
        // Fetch the question based on questionId and display it
        const questionText = await fetchQuestion(questionId);
        document.getElementById('question-container').innerHTML += questionText;

        // Fetch and display comments for the question
        await fetchAndDisplayComments(questionId);
        
    }

    // Call displayQuestion when the page loads
    window.onload = displayQuestion;


</script>

<div id="errorMessage"></div>
<form action= "javascript:reply_api()">
<div id="answer-question-box">
    <span>Answer this Question</span><br>
    <input type="text" id="answer-question-input" placeholder="Enter your response here">
    <button id="response-button" style="background-color: #c5000c; color: white;" >Reply</button>

</div>
</form>
</body>
</html>
