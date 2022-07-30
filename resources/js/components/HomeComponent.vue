<template>
    <div>
        <NavbarComponent />
        <SidebarComponent v-if="!recordInSession" />
        <div class="main" v-if="!recordInSession">
            <div>
                <div class="breadcrumb">
                    <p>Snapbyte > My Recordings</p>
                </div>
                <div class="headingContent">
                    <h1 class="title">
                        My Recordings
                        <span style="color: #70757a">{{
                            recordings.length
                        }}</span>
                    </h1>
                    <div class="right">
                        <button class="date btn">
                            <i
                                class="fa fa-sort-numeric-asc pr-5"
                                aria-hidden="true"
                            ></i>
                            By Date
                        </button>
                        <button class="filter btn">
                            <i class="fa fa-filter" pr-5 aria-hidden="true"></i>
                            Add filter
                        </button>
                        <button class="request btn">
                            <i
                                class="fa fa-video-camera pr-5"
                                aria-hidden="true"
                            ></i>
                            New Request
                        </button>
                        <button
                            class="recording btn"
                            id="recorder"
                            @click="modalGetter"
                        >
                            <div class="circle mr-5">
                                <span>Rec</span>
                            </div>
                            Start recording
                        </button>
                    </div>
                </div>
                <div style="overflow-x: auto">
                    <table>
                        <tr>
                            <th>Recordings</th>
                            <th>Title</th>
                            <th>Views</th>
                            <th>Size</th>
                            <th>Last Modified</th>
                            <th></th>
                        </tr>
                        <tbody>
                            <tr
                                v-for="recording in recordings"
                                :key="recording.id"
                            >
                                <td>
                                    <img
                                        :src="'/img/' + recording.imageUrl"
                                        class="recordImg"
                                        alt="lake recordings"
                                    />
                                </td>
                                <td>
                                    <p class="m-0 title">
                                        {{ recording.title }}
                                    </p>
                                    <p class="m-0 subtitle">
                                        {{ recording.description }}
                                    </p>
                                    <p class="m-0 subtitle">
                                        if the user has added it
                                    </p>
                                </td>
                                <td>{{ recording.views }}</td>
                                <td>{{ recording.size }}</td>
                                <td>
                                    {{ moment(recording.updated_at).fromNow() }}
                                </td>
                                <td>
                                    <i
                                        class="fa fa-ellipsis-h"
                                        aria-hidden="true"
                                    ></i>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
            <div id="recordModal" class="modal">
                <!-- Modal content -->
                <div class="modal-content">
                    <div class="modal-header">
                        <h4>New Recording</h4>
                        <span class="close" @click="closeMe">&times;</span>
                    </div>
                    <div class="modal-body">
                        <label for="location" class="label"
                            >Save the recording in</label
                        >
                        <select class="selectCustom" v-model="project">
                            <option disabled selected value="-">
                                Select a project
                            </option>
                            <option value="1">SD Card</option>
                            <option value="2">Cloud</option>
                            <option value="3">Phone</option>
                        </select>
                        <p v-if="projectError" class="error projectError">
                            {{ projectError }}
                        </p>

                        <div class="flexed">
                            <h5>Record screen</h5>
                            <label class="switch">
                                <input type="checkbox" v-model="screen" />
                                <span class="slider round"></span>
                            </label>
                        </div>
                        <div class="flexed">
                            <h5>Record camera</h5>
                            <label class="switch">
                                <input type="checkbox" v-model="camera" />
                                <span class="slider round"></span>
                            </label>
                        </div>
                        <div class="flexed">
                            <h5>Record mic</h5>
                            <label class="switch">
                                <input type="checkbox" v-model="mic" />
                                <span class="slider round"> </span>
                            </label>
                        </div>
                        <p v-if="peripError" class="error">{{ peripError }}</p>
                        <div class="startContainer">
                            <button class="btn stert" @click="startRecording">
                                Start recording
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div v-else class="recordingBg">
            <div class="content">
                <div class="breadcrumb">
                    <p>
                        <i class="fa fa-stop-circle-o" aria-hidden="true"></i>
                        Live Preview
                    </p>
                </div>
                <div class="recordCenter"></div>
            </div>
            <div class="startContainer">
                <button class="btn stert" disabled>Start recording</button>
            </div>
        </div>
    </div>
</template>

<script>
    import NavbarComponent from "./NavbarComponent.vue";
    import SidebarComponent from "./SidebarComponent.vue";
    import recordings from "../../recordings";
    import moment from "moment";

    let mediaRecorder;
    export default {
        data() {
            return {
                recordings: recordings,
                project: "-",
                screen: false,
                camera: false,
                mic: false,
                projectError: "",
                peripError: "",
                recordInSession: false,
            };
        },
        methods: {
            closeMe() {
                var modal = document.getElementById("recordModal");
                modal.style.display = "none";
            },
            modalGetter() {
                var modal = document.getElementById("recordModal");
                modal.style.display = "block";
            },
            moment(arg) {
                return moment(arg);
            },
            startRecording() {
                var modal = document.getElementById("recordModal");
                this.validateData();
                if (!this.projectError && !this.peripError) {
                    // Run the permission
                    navigator.mediaDevices
                        .getUserMedia({ audio: this.mic, video: this.camera })
                        .then(this.handleSuccess);
                    this.recordInSession = true;
                    modal.style.display = "none";
                    let body = document.getElementById("rootBody");
                    this.recordInSession
                        ? ((body.style.backgroundColor = "#f5fdff"),
                          (body.style.height = "100%"))
                        : null;
                }
            },
            validateData() {
                this.projectError = "";
                this.peripError = "";
                if (this.project == "-") {
                    this.projectError = "Please select a project";
                }
                if (
                    this.screen == false &&
                    this.camera == false &&
                    this.mic == false
                ) {
                    this.peripError = "Please select one or two peripherals to use";
                }
            },
        },
        mounted() {
            var modal = document.getElementById("recordModal");
            window.onclick = function (event) {
                if (event.target == modal) {
                    modal.style.display = "none";
                }
            };
            let body = document.getElementById("rootBody");
            this.recordInSession
                ? ((body.style.backgroundColor = "#f5fdff"),
                  (body.style.height = "100%"))
                : null;
        },
        components: { NavbarComponent, SidebarComponent },
    };
</script>
<style>
    .error {
        margin: 0;
        padding-left: 10px;
        font-size: 0.8em;
        color: red;
    }
    .projectError {
        margin-top: -20px;
    }
    .fa-ellipsis-h {
        font-size: 1.2em;
    }
    .breadcrumb {
        font-size: 12px;
        color: #70757a;
    }
    .btn {
        padding: 10px 15px;
        font-size: 14px;
        text-transform: capitalize;
        margin: 10px;
        border-radius: 20px;
        background-color: #fff;
        border: 1px solid rgb(242, 245, 245);
    }
    .btn:hover {
        cursor: pointer;
    }
    button.request {
        background-color: #0dabd8;
        color: #fff;
    }
    button.recording {
        background-color: #ef5350;
        color: #fff;
    }
    .pr-5 {
        padding-right: 5px;
    }
    .mr-5 {
        margin-right: 5px;
    }
    .circle {
        height: 15px;
        width: 15px;
        background-color: #fff;
        line-height: 15px;
        border-radius: 15px;
        color: black;
        display: inline-block;
        font-size: 0.5em;
        padding: 2px;
    }
    .recordImg {
        width: 180px;
        height: 90px;
        border-radius: 5px;
    }
    table {
        border-collapse: collapse;
        border-spacing: 0;
        width: 100%;
        border: none;
    }
    th,
    td {
        text-align: left;
        padding: 8px;
    }
    .m-0 {
        margin: 0;
    }
    @media screen and (min-width: 1200px) {
        td:first-child {
            width: 15%;
        }
        .headingContent {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
    }
    .title {
        font-weight: 700;
    }
    .subtitle {
        color: #888e91;
    }
    .modal-content {
        width: 25%;
        border-radius: 10px;
        padding-top: 5px;
    }
    .modal-header {
        display: flex;
        justify-content: space-between;
        border-bottom: 1px solid rgb(215, 218, 218);
        padding: 0 20px 5px 20px;
        align-items: center;
    }
    .modal-body {
        padding: 5px 20px 20px 20px;
    }
    .label {
        display: block;
        margin: 10px 10px 10px 0;
    }
    .selectCustom {
        width: 100%;
        height: 40px;
        border-radius: 5px;
        font-size: 1em;
        color: rgb(93, 95, 95);
        padding: 0.5rem 1rem;
        border: none;
        outline: none;
        appearance: none;
        background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
        background-repeat: no-repeat;
        background-position: right 1rem center;
        background-size: 1em;
        margin-bottom: 25px;
    }
    .flexed h5,
    h4 {
        margin: 0.9rem;
    }
    .stert {
        background-color: #0dabd8;
        color: #fff;
        padding-left: 50px;
        padding-right: 50px;
    }
    .startContainer {
        display: flex;
        justify-content: center;
    }

    .fa-stop-circle-o {
        color: rgb(240, 162, 162);
        font-size: 1.2em;
    }

    .btn:disabled {
        opacity: 0.4;
        padding-left: 20px;
        padding-right: 20px;
    }
    @media screen and (min-width: 1200px) {
        .content {
            margin-top: 75px;
            padding-top: 24px;
            margin-left: 220px;
        }
        .recordCenter {
            width: 81%;
            height: 70vh;
            background-color: #1e3246;
            border-radius: 10px;
        }
    }
    @media screen and (max-width: 600px) {
        .content {
            padding-top: 74px;
        }
        .recordCenter {
            width: 90%;
            height: 70vh;
            background-color: #1e3246;
            border-radius: 10px;
            margin: 1px 15px 1px 15px;
        }
        .content .breadcrumb {
            padding-left: 20px;
        }
    }
</style>