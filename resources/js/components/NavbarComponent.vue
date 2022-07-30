<template>
    <div>
        <div class="nav" id="nav">
            <div class="flexed ml-10">
                <img
                    src="/img/logo.jpg"
                    width="25px"
                    height="25px"
                    alt="brand logo"
                    class="large"
                />
                <a href="#" class="brandLogo">Adilo</a>
            </div>
            <div>
                <a href="#news" class="navLinks">Projects</a>
                <div class="dropdown navLinks active">
                    <button class="dropbtn">
                        Tools & App
                        <i class="fa fa-angle-down"></i>
                    </button>
                    <div class="dropdown-content">
                        <a href="#">Link 1</a>
                        <a href="#">Link 2</a>
                        <a href="#">Link 3</a>
                    </div>
                </div>
                <a href="#channels" class="navLinks">Channels</a>
                <a href="#contact" class="navLinks">Contacts</a>
                <a href="#analytics" class="navLinks">Analytics</a>
                <a href="#settings" class="navLinks">Settings</a>
                <a href="#about" class="navLinks">About</a>
            </div>
            <div class="right">
                <a href="#help" class="navLinks black">Help</a>
                <div class="flexed divider">
                    <p>
                        <i
                            class="fa fa-user-circle-o paddingLeft"
                            aria-hidden="true"
                        ></i>
                    </p>
                    <div>
                        <p>Cody Fisher</p>
                        <p>CodyFisher@gmail.com</p>
                    </div>
                </div>
            </div>
            <a href="javascript:void(0);" class="icon" @click="mobileDrop"
                >&#9776;</a
            >
        </div>
    </div>
</template>
<script>
    export default {
        setup() {},
        methods: {
            mobileDrop() {
                /* Toggle between adding and removing the "responsive" class to nav when the user clicks on the icon */
                var x = document.getElementById("nav");
                if (x.className === "nav") {
                    x.className += " responsive";
                } else {
                    x.className = "nav";
                }
            },
            switchLinks() {
                var nav = document.getElementById("nav");
                var links = nav.getElementsByClassName("navLinks");
                for (var i = 0; i < links.length; i++) {
                    links[i].addEventListener("click", function () {
                        var current = document.getElementsByClassName("active");
                        // if (current.length > 0) {
                        current[0].className = current[0].className.replace(
                            " active",
                            ""
                        );
                        // }
                        this.className += " active";
                    });
                }
            },
        },

        mounted() {
            this.switchLinks();
        },
    };
</script>
<style>
    .flexed {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
</style>
<style scoped>
    .ml-10 {
        margin-left: 20px;
    }

    .paddingLeft {
        padding-right: 10px;
    }

    .black {
        color: rgb(31, 30, 30) !important;
    }

    a.brandLogo {
        font-size: 20px !important;
        font-weight: 900;
        padding: 20px !important;
    }

    .divider {
        border-left: 1px solid black;
        padding: 0 20px 0 20px;
        margin: 10px;
    }

    .divider p {
        margin: 0;
        font-size: 15px;
    }

    .divider div p:nth-child(1) {
        font-weight: bold;
    }

    .divider div p:last-child {
        color: #8a8787;
    }

    .divider p > i {
        font-size: 2.5rem;
    }

    /* Style the links inside the navigation bar */
    .nav a {
        float: left;
        display: block;
        color: #8a8787;
        text-align: center;
        padding: 24px 16px;
        text-decoration: none;
        font-size: 15px;
        font-weight: 700;
    }

    /* Add an active class to highlight the current page */
    .active {
        color: rgb(7, 6, 6) !important;
        border-top: 3px solid #0dabd8;
        padding: 20px 16px !important;
    }

    /* Hide the link that should open and close the nav on small screens */
    .nav .icon {
        display: none;
    }

    /* Dropdown container - needed to position the dropdown content */
    .dropdown {
        float: left;
        overflow: hidden;
    }

    /* Style the dropdown button to fit inside the nav */
    .dropdown .dropbtn {
        font-size: 17px;
        border: none;
        outline: none;
        color: #8a8787;
        padding: 24px 16px;
        background-color: inherit;
        font-family: inherit;
        margin: 0;
        font-size: 15px;
        font-weight: 700;
    }

    .dropdown.active .dropbtn {
        padding: 1px 16px;
    }

    /* Style the dropdown content (hidden by default) */
    .dropdown-content {
        display: none;
        position: absolute;
        background-color: #f9f9f9;
        min-width: 160px;
        box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
        z-index: 1;
    }

    /* Style the links inside the dropdown */
    .dropdown-content a {
        float: none;
        color: black;
        padding: 12px 16px;
        text-decoration: none;
        display: block;
        text-align: left;
    }

    /* Add a dark background on nav links and the dropdown button on hover */
    .nav a:hover,
    .dropdown:hover .dropbtn {
        color: #0dabd8;
    }

    /* Add a grey background to dropdown links on hover */
    .dropdown-content a:hover {
        background-color: #ddd;
        color: black;
    }

    /* Show the dropdown menu when the user moves the mouse over the dropdown button */
    .dropdown:hover .dropdown-content {
        display: block;
        position: fixed
    }

    @media screen and (min-width: 1200px) {
        .nav {
            overflow: hidden;
            display: flex;
            justify-content: space-between;
            border-bottom: 1px solid rgb(219, 210, 210);
            position: fixed;
            width: 100%;
            background-color: white;
            z-index: 1;
            margin-top: 0;
            top: 0;
        }
    }

    /* When the screen is less than 600 pixels wide, hide all links, except for the first one ("Home"). Show the link that contains should open and close the nav (.icon) */
    @media screen and (max-width: 600px) {
        .nav div:not(:first-child),
        .dropdown .dropbtn {
            display: none;
        }
        .nav a.icon {
            float: right;
            display: block;
        }
        .large {
            display: none;
        }
        .flexed.ml-10 {
            display: block;
            margin-left: 0;
        }
    }

    /* The "responsive" class is added to the nav with JavaScript when the user clicks on the icon. This class makes the nav look good on small screens (display the links vertically instead of horizontally) */
    @media screen and (max-width: 600px) {
        .nav.responsive {
            position: relative;
        }

        .nav.responsive a.icon {
            position: absolute;
            right: 0;
            top: 0;
        }
        .nav.responsive a,
        .nav.responsive div {
            float: none;
            display: block;
            text-align: left;
        }
        .nav.responsive div > a {
            padding: 12px 16px;
        }
        .nav.responsive .dropdown {
            float: none;
        }
        .nav.responsive .dropdown-content {
            position: relative;
        }
        .nav.responsive .dropdown .dropbtn {
            display: block;
            width: 100%;
            text-align: left;
        }
        .nav.responsive .dropdown.active {
            padding: 20px 1px !important;
        }

        .nav.responsive .div.dropdown-content {
            display: none;
        }
        .flexed.divider {
            display: flex !important;
            justify-content: flex-start;
            border: none;
            margin-left: 0;
        }
    }
</style>