# Open-Space-Infinite-Password-Manager


<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PassX - Your Personal Password Manager</title>
    <link rel="stylesheet" href="style.css">
</head>

<body>
    <nav>
        <div class="logo">Open Space Infinite</div>
        <ul>
            <li>Home</li>
            <li>About</li>
            <li>Contact</li>
        </ul>
    </nav>
    <div class="container">
        <h1>Password Manager</h1>
        <p>We're thrilled to have you here. Your digital life contains a myriad of passwords, and we know how challenging it can be to manage them all. That's why we're here to make it easy for you.</p>
        <h2>Your Passwords <span id="alert">(Copied!)</span></h2>
        <table>
            <tr>
                <th>Service</th>
                <th>Username</th>
                <th>Password</th>
                <th>Delete</th>
            </tr>

        </table>

        <h2>Add a Password</h2>
        <form action="/submit" method="post">

            <!-- Text input for Service -->
            <label for="website">Service:</label>
            <input type="text" id="Service" name="Service" required>
            <br><br>

            <!-- Text input for username -->
            <label for="username">Username:</label>
            <input type="text" id="username" name="username" required>
            <br><br>

            <!-- Password input -->
            <label for="password">Password:</label>
            <input type="password" id="password" name="password" required>
            <br>
            <!-- Submit button -->
            <button class="btn" type="submit">Submit</button>
        </form>
    </div>
    <script src="script.js"></script>

    <head>
        <style>
          /* Your CSS code here */
          @import url('https://fonts.googleapis.com/css2?family=Noto+Sans:ital,wght@0,700;1,300&family=Poppins:wght@300;400;600&display=swap');
* {
    margin: 0;
    padding: 0;
    font-family: 'Noto Sans', sans-serif;
    font-family: 'Poppins', sans-serif;
}

nav {
    background-color: black;
    color: white;
    padding: 12px 3px;
    display: flex;
    justify-content: space-between;
}

.logo {
    margin: 0 23px;
    font-weight: 800;
    font-size: 25px;
    cursor: pointer;
}

ul {
    display: flex;
    margin: 0 23px;
    align-items: center;
}

ul>li {
    list-style: none;
    margin: 0 13px;
    cursor: pointer;
}

ul>li:hover {
    color: white;
}

table,
td,
tr {
    border: 2px solid black;
    border-collapse: collapse;
    padding: 5px 13px;
}

.container {
    max-width: 80vw;
    margin: 23px auto;
}

h1,
h2,
h3 {
    margin: 23px 0;
}

.btn {
    padding: 8px 17px;
    background: black;
    color: white;
    font-weight: 900;
    border: 2px solid gray;
    border-radius: 8px;
    margin: 25px 0;
    cursor: pointer;
}

.btnsm {
    padding: 8px 17px;
    background: black;
    color: white;
    font-weight: 900;
    border: 2px solid gray;
    border-radius: 8px;
    cursor: pointer;
}

img {
    cursor: pointer;
    position: relative;
    bottom: 7px;
    width: 15px;
    height: 13px;
}

#alert {
    display: none;
}

        </style>
        <script>
          // Your JavaScript code here
          function maskPassword(pass) {
    let str = ""
    for (let index = 0; index < pass.length; index++) {
        str += "*"
    }
    return str
}

function copyText(txt) {
    navigator.clipboard.writeText(txt).then(
        () => {
            /* clipboard successfully set */
            document.getElementById("alert").style.display = "inline"
            setTimeout(() => {
                document.getElementById("alert").style.display = "none"
            }, 2000);

        },
        () => {
            /* clipboard write failed */
            alert("Clipboard copying failed")
        },
    );
}

const deletePassword = (Service) => {
    let data = localStorage.getItem("passwords")
    let arr = JSON.parse(data);
    arrUpdated = arr.filter((e) => {
        return e.Service != Service
    })
    localStorage.setItem("passwords", JSON.stringify(arrUpdated))
    alert(`Successfully deleted ${Service}'s password`)
    showPasswords()

}

// Logic to fill the table
const showPasswords = () => {
    let tb = document.querySelector("table")
    let data = localStorage.getItem("passwords")
    if (data == null || JSON.parse(data).length == 0) {
        tb.innerHTML = "No Data To Show"
    } else {
        tb.innerHTML = `<tr>
        <th>Service</th>
        <th>Username</th>
        <th>Password</th>
        <th>Delete</th>
    </tr> `
        let arr = JSON.parse(data);
        let str = ""
        for (let index = 0; index < arr.length; index++) {
            const element = arr[index];

            str += `<tr>
    <td>${element.Service} <img onclick="copyText('${element.Service}')" src="./copy.svg" alt="Copy Button" width="30" width="10" height="10">
    </td>
    <td>${element.username} <img onclick="copyText('${element.username}')" src="./copy.svg" alt="Copy Button" width="30" width="10" height="10">
    </td>
    <td>${maskPassword(element.password)} <img onclick="copyText('${element.password}')" src="./copy.svg" alt="Copy Button" width="10" width="10" height="10">
    </td>
    <td><button class="btnsm" onclick="deletePassword('${element.Service}')">Delete</button></td>
        </tr>`
        }
        tb.innerHTML = tb.innerHTML + str

    }
    Service.value = ""
    username.value = ""
    password.value = ""
}

console.log("Working");
showPasswords()
document.querySelector(".btn").addEventListener("click", (e) => {
    e.preventDefault()
    console.log("Clicked....")
    console.log(username.value, password.value)
    let passwords = localStorage.getItem("passwords")
    console.log(passwords)
    if (passwords == null) {
        let json = []
        json.push({
            Service: Service.value,
            username: username.value,
            password: password.value
        })
        alert("Password Saved");
        localStorage.setItem("passwords", JSON.stringify(json))
    } else {
        let json = JSON.parse(localStorage.getItem("passwords"))
        json.push({
            Service: Service.value,
            username: username.value,
            password: password.value
        })
        alert("Password Saved");
        localStorage.setItem("passwords", JSON.stringify(json))
    }
    showPasswords()
})


        </script>
      </head>

</body>

</html>
