const  mysql = require('mysql');
const express = require('express');
const bodyParser = require('body-parser');

const app = express();
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));

app.post('/ussd', (req, res) => {
    console.log('Connect');
    // Read the variables sent via POST from our API
    const {
        sessionId,
        serviceCode,
        phoneNumber,
        text,
    } = req.body;
    console.log(req.body);

    let response = '';

    if (text == '') {
        // This is the first request. Note how we start the response with CON
       let name = get_user(req.body.phoneNumber).name;
        response = `CON  Hello
        1. My account
        2. My phone number`;
        //is number registered in DB
    } else if ( text == '1') {
        // Business logic for first level response
        response = `CON Choose account information you want to view
        1. Account number`;
    } else if ( text == '2') {
        // Business logic for first level response
        // This is a terminal request. Note how we start the response with END
        response = `END Your phone number is ${phoneNumber}`;
    } else if ( text == '1*1') {
        // This is a second level response where the user selected 1 in the first instance
        const accountNumber = 'ACC100101';
        // This is a terminal request. Note how we start the response with END
        response = `END Your account number is ${accountNumber}`;
    }
    console.log('final');

    // Send the response back to the API
    res.send(response);
});
app.get('/ussd', (req, res) => {
    console.log('Connect');
    // Read the variables sent via POST from our API
    const {
        sessionId,
        serviceCode,
        phoneNumber,
        text,
    } = req.body;

    let response = '';

    if (text == '') {
        // This is the first request. Note how we start the response with CON
        response = `CON What would you like to check
        1. My account
        2. My phone number`;
    } else if ( text == '1') {
        // Business logic for first level response
        response = `CON Choose account information you want to view
        1. Account number`;
    } else if ( text == '2') {
        // Business logic for first level response
        // This is a terminal request. Note how we start the response with END
        response = `END Your phone number is ${phoneNumber}`;
    } else if ( text == '1*1') {
        // This is a second level response where the user selected 1 in the first instance
        const accountNumber = 'ACC100101';
        // This is a terminal request. Note how we start the response with END
        response = `END Your account number is ${accountNumber}`;
    }

    // Send the response back to the API
    res.set('Content-Type: text/plain');
    res.send(response);
});
app.listen(80, () => {
    console.log(`USSD app listening on port 80`)
  });
function get_user (phone){con = mysql.createConnection({
    host: "localhost",
    user: "root",
    password: "password"
  });
  con.connect(function(err) {
    if (err) throw err;
    con.query("SELECT * FROM users WHERE phone ="+ phone, function (err, result) {
      if (err) throw err;
      console.log(result);
      return(result)
    });
  });
  }
  
function get_appointments(uid)
