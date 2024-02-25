---
layout: default
title: Your Profile
course: compsci
permalink: /profile
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            min-height: 100vh;
            background-color: #f5f5f5;
        }
        .container {
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            width: 80%;
            max-width: 400px;
            text-align: center;
        }
        form {
            display: flex;
            flex-direction: column;
        }
        input[type="text"], input[type="date"] {
            padding: 10px;
            margin: 5px 0 20px;
            border-radius: 5px;
            border: 1px solid #ddd;
            width: calc(100% - 22px);
        }
        button {
            background-color: #ff0000;
            color: white;
            padding: 10px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #ff2f00;
        }
        h2, h3 {
            color: #333;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- <div id="profile-info">
            <p>Name: <span id="userName"></span></p>
            <p>Date of Birth: <span id="userDOB"></span></p>
            <p>Username: <span id="userUID"></span></p>
        </div> -->
        <h3>Edit Profile</h3>
        <form action = "javascript:updateUserProfile()">
            <label for="name">Name:</label>
            <input type="text" id="name" name="name"><br>
            <label for="uid">Username:</label>
            <input type="text" id="uid" name="username"><br>
            <label for="dob">Date of Birth:</label>
            <input type="date" id="dob" name="dob"><br>  
            <button>Update Profile</button>
        </form>
<button id="deleteUser">Delete Account</button>
    </div>

<script>
    const name = localStorage.getItem("userUid"); 
    // const dob = localStorage.getItem("userDOB");  
    // document.getElementById("userName").value = name;
    // document.getElementById("userDOB").textContent = dob; 
    //  document.getElementById("userUID").textContent = uid 
    let intId = 48
    function dataGet(){
        var myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");
        // var raw = JSON.stringify({
        //   "name": name,
        //   "uid": uid,
        //   "id": id,
        //   "dob": dob
        // });
        var requestOptions = {
            method: 'GET',
            headers: myHeaders,
            redirect: 'follow'
        };
        <!-- const userId = id -->
        fetch("http://127.0.0.1:8086/api/users/?uid=" + name, requestOptions)
         .then(response => {
            if (response.ok) {
                //console.log(enteredTopic + " has been searched");
                return response.json(); // Parse the JSON in the response body
            } else {
                console.error("Search failed");
                const errorMessageDiv = document.getElementById('errorMessage');
                errorMessageDiv.innerHTML = '<label style="color: red;">Search Failed</label>';
                throw new Error('Search failed');
            }
        })
        .then(data => {
            console.log(data); // You can see your fetched data here
            intId = data.id
            const dob = data.dob
            const username = data.uid
            const firstName = data.name
            // const intId = data.id
            document.getElementById('name').value = firstName
            document.getElementById('dob').value = dob
            document.getElementById('uid').value = username

        })
        .catch(error => {
            // Handle any errors that occurred during the fetch() or in the promise chain
            console.error('Error:', error);
        });
    }
    
    
        window.onload = dataGet;
    </script>
</body>
</html>
