<template>
    <div>
        <div class="sidebar" id="sidebar">
            <a href="#home" class="active sidebarLinks"
                ><i
                    class="fa fa-film"
                    aria-hidden="true"
                    style="padding-right: 25px"
                ></i>
                My Recordings</a
            >
            <a href="#services" class="sidebarLinks"
                ><i
                    class="fa fa-share-alt"
                    aria-hidden="true"
                    style="padding-right: 25px; font-size: 1.3rem"
                ></i>
                Requested</a
            >
        </div>
    </div>
</template>
<script>
    export default {
        methods: {
            switchLinkSidbar() {
                var sidenav = document.getElementById("sidebar");
                var links = sidenav.getElementsByClassName("sidebarLinks");
                for (var i = 0; i < links.length; i++) {
                    links[i].addEventListener("click", function () {
                        var current = document.getElementsByClassName(
                            "active sidebarLinks"
                        );
                        current[0].className = current[0].className.replace(
                            "active",
                            ""
                        );

                        this.className += " active";
                    });
                }
            },
        },

        mounted() {
            this.switchLinkSidbar();
        },
    };
</script>
<style scoped>
    .sidebar {
        height: 100%;
        width: 220px;
        position: fixed;
        z-index: 1;
        left: 0;
        background-color: #f9fbfc;
        overflow-x: hidden;
        padding-top: 16px;
    }

    .active {
        background-color: #e2e5ed;
        border-radius: 5px;
        line-height: 2.5;
        height: 50px;
    }

    /* Style sidebar links */
    .sidebar a {
        padding: 6px 8px 20px 16px;
        text-decoration: none;
        font-size: 15px;
        color: #5a5858;
        display: block;
        margin: 15px;
    }

    /* Style links on mouse-over */
    .sidebar a:hover {
        color: #474949;
    }

    /* Add media queries for small screens (when the height of the screen is less than 450px, add a smaller padding and font-size) */
    @media screen and (max-height: 450px) {
        .sidebar {
            padding-top: 15px;
        }
        .sidebar a {
            font-size: 18px;
        }
    }
</style>
<style>
    @media screen and (max-width: 600px) {
        .sidebar {
            display: none;
        }
        .main {
            margin: 0;
            padding: 70px 10px;
        }
    }
    @media screen and (min-width: 1200px) {
        .main {
            margin-left: 290px;
            margin-top:75px;
            padding: 6px 10px;
        }
    }
    @media screen and (min-width: 640px) {
        
    }
</style>