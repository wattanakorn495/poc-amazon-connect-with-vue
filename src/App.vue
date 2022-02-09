<template>
    <div id="app">
        <button @click="onLogin">login</button>
        <button @click="onOpenChat">open chat</button>
        <div
            :class="isInit ? `chat` : `hide-chat`"
            id="container-div"
            style="width: 400px; height: 600px"
        ></div>
    </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import "amazon-connect-streams";
export default {
    name: "App",
    components: {
        // HelloWorld
    },
    data() {
        return {
            showLogin: false,
            timer: null,
            windowRef: null,
            isInit: false,
        };
    },
    mounted() {
        console.log("mounted");
        // var containerDiv = document.getElementById("container-div");
        // var instanceURL = "https://villa-dev.my.connect.aws/ccp-v2/";
        // window.connect.core.initCCP(containerDiv, {
        //     ccpUrl: instanceURL, // REQUIRED
        //     loginPopup: false, // optional, defaults to `true`
        //     loginPopupAutoClose: true, // optional, defaults to `false`
        //     loginOptions: {
        //         // optional, if provided opens login in new window
        //         autoClose: true, // optional, defaults to `false`
        //         height: 600, // optional, defaults to 578
        //         width: 400, // optional, defaults to 433
        //         top: 0, // optional, defaults to 0
        //         left: 0, // optional, defaults to 0
        //     },
        //     region: "ap-southeast-1", // REQUIRED for `CHAT`, optional otherwise
        //     softphone: {
        //         // optional, defaults below apply if not provided
        //         allowFramedSoftphone: true, // optional, defaults to false
        //         disableRingtone: false, // optional, defaults to false
        //         ringtoneUrl: "./ringtone.mp3", // optional, defaults to CCP’s default ringtone if a falsy value is set
        //     },
        //     pageOptions: {
        //         //optional
        //         enableAudioDeviceSettings: false, //optional, defaults to 'false'
        //         enablePhoneTypeSettings: false, //optional, defaults to 'true'
        //     },
        //     ccpAckTimeout: 25000, //optional, defaults to 3000 (ms)
        //     ccpSynTimeout: 30000, //optional, defaults to 1000 (ms)
        //     ccpLoadTimeout: 10000, //optional, defaults to 5000 (ms)
        // });

        // window.connect.EventType.AUTH_FAIL

        // const bus = window.connect.core.getEventBus();
        // // console.log({bus}, window.connect.EventType);
        // bus.subscribe(window.connect.EventType.ACK_TIMEOUT, () => {
        //   console.log("auth fail");
        //   this.showLogin = true;
        // })
        // eventBus.subscribe(window.connect.EventType., () => {
        //     console.log("Logged out");
        //     // Do stuff...
        //     this.isInit = false;
        // });

        // setInterval(() => {
        //     console.log(window.connect);
        //     // if(window.connect.core.initialized)
        // }, 1000);
    },
    methods: {
        onLogin() {
            this.windowRef = window.open(
                "https://villa-dev.my.connect.aws/ccp-v2/",
                "",
                "width=600,height=400,left=200,top=200"
            );

            var timer = setInterval(() => {
                if (this.windowRef.closed) {
                    clearInterval(timer);
                    var containerDiv = document.getElementById("container-div");
                    var instanceURL =
                        "https://villa-dev.my.connect.aws/ccp-v2/";
                    window.connect.core.initCCP(containerDiv, {
                        ccpUrl: instanceURL, // REQUIRED
                        loginPopup: false, // optional, defaults to `true`
                        loginPopupAutoClose: true, // optional, defaults to `false`
                        loginOptions: {
                            // optional, if provided opens login in new window
                            autoClose: true, // optional, defaults to `false`
                            height: 600, // optional, defaults to 578
                            width: 400, // optional, defaults to 433
                            top: 0, // optional, defaults to 0
                            left: 0, // optional, defaults to 0
                        },
                        region: "ap-southeast-1", // REQUIRED for `CHAT`, optional otherwise
                        softphone: {
                            // optional, defaults below apply if not provided
                            allowFramedSoftphone: true, // optional, defaults to false
                            disableRingtone: false, // optional, defaults to false
                            ringtoneUrl: "./ringtone.mp3", // optional, defaults to CCP’s default ringtone if a falsy value is set
                        },
                        pageOptions: {
                            //optional
                            enableAudioDeviceSettings: false, //optional, defaults to 'false'
                            enablePhoneTypeSettings: false, //optional, defaults to 'true'
                        },
                        ccpAckTimeout: 25000, //optional, defaults to 3000 (ms)
                        ccpSynTimeout: 30000, //optional, defaults to 1000 (ms)
                        ccpLoadTimeout: 10000, //optional, defaults to 5000 (ms)
                    });
                }

                window.connect.core.onInitialized(() => {
                    console.log("onInitialized");
                    this.isInit = true;
                });

                window.connect.core.onAuthFail(function () {
                    console.log("onAuthFail");
                });

                window.connect.core.onAccessDenied(function () {
                    console.log("onAccessDenied");
                });

                window.connect.agent(function (agent) {
                    console.log(agent);
                    var state = agent.getAgentStates();
                    agent.setState(state[0]);
                });

                const eventBus = window.connect.core.getEventBus();
                eventBus.subscribe(window.connect.EventType.TERMINATED, () => {
                    console.log("Logged out");
                    // Do stuff...
                    this.isInit = false;
                });
            }, 1000);
        },
        onOpenChat() {
            // window.open("https://villa-dev.my.connect.aws/ccp-v2/");
        },
    },
    beforeDestroy() {
        if (this.timer) {
            clearInterval(this.timer);
        }
    },
};
</script>

<style>
/* #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
} */

.hide-chat {
    display: none;
}
</style>
