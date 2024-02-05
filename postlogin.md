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
</div>

