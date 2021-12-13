<template>
    <div id="app">
        <button v-if="showLogin" @click="onLogin">login</button>
        <div v-else id="container-div" style="width: 400px; height: 600px"></div>
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
    data(){
      return {
        showLogin: false
      }
    },
    mounted() {
        console.log("mounted");
        var containerDiv = document.getElementById("container-div");

        // var instanceURL = "https://my-instance-domain.awsapps.com/connect/ccp-v2/";
        var instanceURL = "https://villa-dev.my.connect.aws/ccp-v2/";
        window.connect.core.initCCP(containerDiv, {
            ccpUrl: instanceURL, // REQUIRED
            loginPopup: true, // optional, defaults to `true`
            loginPopupAutoClose: true, // optional, defaults to `false`
            loginOptions: {
                // optional, if provided opens login in new window
                autoClose: true, // optional, defaults to `false`
                height: 600, // optional, defaults to 578
                width: 400, // optional, defaults to 433
                top: 0, // optional, defaults to 0
                left: 0, // optional, defaults to 0
            },
            region: "eu-central-1", // REQUIRED for `CHAT`, optional otherwise
            softphone: {
                // optional, defaults below apply if not provided
                allowFramedSoftphone: true, // optional, defaults to false
                disableRingtone: false, // optional, defaults to false
                ringtoneUrl: "./ringtone.mp3", // optional, defaults to CCP’s default ringtone if a falsy value is set
            },
            pageOptions: {
                //optional
                enableAudioDeviceSettings: false, //optional, defaults to 'false'
                enablePhoneTypeSettings: true, //optional, defaults to 'true'
            },
            ccpAckTimeout: 25000, //optional, defaults to 3000 (ms)
            ccpSynTimeout: 30000, //optional, defaults to 1000 (ms)
            ccpLoadTimeout: 10000, //optional, defaults to 5000 (ms)
        });

        // window.connect.EventType.AUTH_FAIL

        const bus = window.connect.core.getEventBus();
        console.log({bus}, window.connect.EventType);
        bus.subscribe(window.connect.EventType.ACK_TIMEOUT, () => {
          console.log("auth fail");
          this.showLogin = true;
        })

        // window.connect.core.onAuthFail(function () {
        //     console.log("auth fail");
        // });
    },
    methods: {
      onLogin(){
        this.windowRef = window.open("https://villa-dev.my.connect.aws/ccp-v2/", "", "width=600,height=400,left=200,top=200");
      }
    }
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
</style>
