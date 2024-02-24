<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Question Details</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            flex-direction: column;
            font-family: Arial, sans-serif;
        }
        
                .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 20px;
            background: #fff;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .header-left {
            font-size: 0.9em;
            color: #666;
        }

        .header-right a {
            margin-left: 20px;
            text-decoration: none;
            color: #333;
            font-weight: bold;
        }

        .container {
            width: 80%; /* Adjust the width as needed */
            max-width: 600px; /* Set a max-width for larger screens */
            margin: auto;
        }

        #search-input, #ask-question-input {
            width: 100%; /* Make the input take the full width of its parent */
            padding: 8px;
            font-size: 16px;
            box-sizing: border-box; /* Include padding in the width calculation */
            margin-bottom: 10px; /* Adds some space below the input */
        }

        #search-button, #submit-button {
            width: 100%; /* Align the button width with the input */
            background-color: #c5000c;
            color: white;
            padding: 10px 0; /* Adjust padding as needed */
            border: none;
            cursor: pointer;
            margin-bottom: 20px; /* Adds some space below the button */
        }

        #search-results, #tableContainer {
            list-style-type: none;
            padding: 0;
        }

        .question-box, #ask-question-box {
            border: 1px solid #ccc;
            padding: 20px;
            border-radius: 5px;
            text-align: left; /* Aligns text to the left inside the centered container */
        }

        .question-box .user-info, .date {
            margin-bottom: 10px;
        }

        .date {
            color: #666;
            font-size: 14px;
        }

        /* This is for the "Want to ask a question?" text */
        .ask-question-label {
            text-align: left; /* Center the label text */
            display: block; /* Make the label a block element to take full width */
            margin-bottom: 5px; /* Space before the input box */
        }

        /* Login and Sign Up links */
        .auth-links {
            text-align: right; /* Aligns the links to the right */
            padding: 10px 20px; /* Padding to position the links */
            background: none;
        }
    </style>
</head>
<body>

<div class="header">
    <div class="header-left">
        CompSci Blogs
        <br>
        August 2023 to June 2024
    </div>
    <div class="header-right">
        <a href="/CollaboraDJAK/login">Log In</a>  <a href="/CollaboraDJAK/signup">Sign Up</a>
        <a href="/CollaboraDJAK/profile" class="profile-button">View your Profile</a>
    </div>
</div>

<div class="container">
    <span style="font-size:3em;">Welcome to Collabora</span>

    <div class="question-box">
        <div class="user-info">
            <span>Name: </span><span id="name-span"></span><br>
            <span>Username: </span><span id="uid-span"></span>
        </div>
        <div class="date" id="post-date"></div>
    </div>

    <form action="javascript:topicSearch()">
        <div id="search">
            <span>Search for a question?</span><br>
            <input type="text" id="search-input" placeholder="Search a topic">
            <button id="search-button">Search</button>
        </div>
    </form>
    <div id="errorMessage"></div>
    <div id="tableContainer"></div>

    <form action="javascript:createPost()">
        <label for="ask-question-input" class="ask-question-label">Want to ask a question?</label>
        <input type="text" id="ask-question-input" placeholder="Enter your question here">
        <button id="submit-button">Submit</button>
    </form>
</div>

<script>
function createTableFromJSON(jsonData) {
    // Create a table
    var table = document.createElement("table");
    var tr = table.insertRow(-1); // Table Row

    // Add table headers
    var headers = ["Questions"];
    for(var i = 0; i < headers.length; i++) {
        var th = document.createElement("th"); // Table Header
        th.innerHTML = headers[i];
        tr.appendChild(th);
    }

    // Add JSON data to the table as rows
    for(var i = 0; i < jsonData.length; i++) {
        tr = table.insertRow(-1);
        for(var j = 0; j < headers.length; j++) {
            var tabCell = tr.insertCell(-1);
            tabCell.innerHTML = "<a href='question?questionId="+jsonData[i]['id']+"'>"+jsonData[i]['note']+"</a>";
        }
    }

    // Finally, add the newly created table with JSON data to a container
    var divContainer = document.getElementById("tableContainer");
    divContainer.innerHTML = "";
    divContainer.appendChild(table);
}

function topicSearch() {
    const enteredTopic = document.getElementById("search-input").value;

    var myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");
    
    var requestOptions = {
        method: 'GET',
        headers: myHeaders,
        redirect: 'follow'
    };

    fetch("http://127.0.0.1:8086/api/post/?searchString=" + enteredTopic, requestOptions)
        .then(response => {
            if (response.ok) {
                console.log(enteredTopic + " has been searched");
                return response.json(); // Parse the JSON in the response body
            } else {
                console.error("Search failed");
                const errorMessageDiv = document.getElementById('errorMessage');
                errorMessageDiv.innerHTML = '<label style="color: red;">Search Failed</label>';
                throw new Error('Search failed');
            }
        })
        .then(data => {
            // Here 'data' is the parsed JSON object from the response body
            console.log(data); // You can see your fetched data here
            createTableFromJSON(data); // Assuming 'createTableFromJSON' expects the JSON data as parameter
        })
        .catch(error => {
            // Handle any errors that occurred during the fetch() or in the promise chain
            console.error('Error:', error);
        });
}

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
const formattedToday = getTodaysDate();
//document.getElementById('post-date').innerHTML = "Posted on: " + formattedToday;

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

    fetch("http://127.0.0.1:8086/api/post/", requestOptions)
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

//const enteredName = localStorage.getItem("enteredName");
//const enteredUid = localStorage.getItem("enteredUid");

        // Update HTML with stored data
//document.getElementById("name-span").textContent = enteredName;
//document.getElementById("uid-span").textContent = enteredUid;
</script>

</body>
</html>
