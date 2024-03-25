# Forms-JavaScript
#JS CODE
let nameEl = document.getElementById("name");
let nameErrMsgEl = document.getElementById("nameErrMsg");

let emailEl = document.getElementById("email");
let emailErrMsgEl = document.getElementById("emailErrMsg");

let workingStatusEl = document.getElementById("status");
let genderMaleEl = document.getElementById("genderMale");
let genderFemaleEl = document.getElementById("genderFemale");

let myFormEl = document.getElementById("myForm");

let formData = {
    name: "",
    email: "",
    status: "Active",
    gender: "Male"
};

nameEl.addEventListener("change", function(event) {
    if (event.target.value === "") {
        nameErrMsgEl.textContent = "Required*";
    } else {
        nameErrMsgEl.textContent = "";
    }

    formData.name = event.target.value;
});

emailEl.addEventListener("change", function(event) {
    if (event.target.value === "") {
        emailErrMsgEl.textContent = "Required*";
    } else {
        emailErrMsgEl.textContent = "";
    }

    formData.email = event.target.value;
});

workingStatusEl.addEventListener("change", function(event) {
    formData.status = event.target.value;
});

genderMaleEl.addEventListener("change", function(event) {
    formData.gender = event.target.value;
});

genderFemaleEl.addEventListener("change", function(event) {
    formData.gender = event.target.value;
});

function validateFormData(formData) {
    let {
        name,
        email
    } = formData;
    if (name === "") {
        nameErrMsgEl.textContent = "Required*";
    }
    if (email === "") {
        emailErrMsgEl.textContent = "Required*";
    }
}

function submitFormData(formData) {
    let options = {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
            Accept: "application/json",
            Authorization: "Bearer 00f3f8fde06120db02b587cc372c3d85510896e899b45774068bb750462acd9f",
        },
        body: JSON.stringify(formData)
    };

    let url = "https://gorest.co.in/public-api/users";

    fetch(url, options)
        .then(function(response) {
            return response.json();
        })
        .then(function(jsonData) {
            console.log(jsonData);
            if (jsonData.code === 422) {
                if (jsonData.data[0].message === "has already been taken") {
                    emailErrMsgEl.textContent = "Email Already Exists";
                }
            }
        });
}

myFormEl.addEventListener("submit", function(event) {
    event.preventDefault();
    validateFormData(formData);
    submitFormData(formData);
});
#CSS CODE
.form-heading {
    font-family: "Roboto";
    font-size: 36px;
    padding-top: 40px;
    padding-bottom: 20px;
}

.error-message {
    color: #dc3545;
    font-family: "Roboto";
    font-size: 14px;
}

.gender-field-heading {
    color: #212529;
    font-size: 18px;
    margin-bottom: 10px;
}
#HTML CODE
<div class="container">
        <h1 class="form-heading">Add User</h1>
        <form id="myForm">
            <div class="mb-3">
                <label for="name">Name</label>
                <input type="text" class="form-control" id="name" />
                <p id="nameErrMsg" class="error-message"></p>
            </div>
            <div class="mb-3">
                <label for="email">Email</label>
                <input type="text" class="form-control" id="email" />
                <p id="emailErrMsg" class="error-message"></p>
            </div>
            <div class="mb-3">
                <label for="status">Working Status</label>
                <select id="status" class="form-control">
                    <option value="Active">Active</option>
                    <option value="Inactive">Inactive</option>
                </select>
            </div>
            <div class="mb-3">
                <h1 class="gender-field-heading">Gender</h1>
                <input type="radio" name="gender" id="genderMale" value="Male" checked />
                <label for="genderMale">Male</label>
                <input type="radio" name="gender" id="genderFemale" value="Female" class="ml-2" />
                <label for="genderFemale">Female</label>
            </div>
            <button class="btn btn-primary">Submit</button>
        </form>
    </div>
