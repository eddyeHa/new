<template lang="html">
	<div class="mainContainer">
  		<div class="subContainer">
    		<div class="content">
      			<img class="poster" src="http://tdgrecords.com/wp-content/uploads/2017/05/Thermo-LaGlotoneria.jpg" alt="" />
    		</div>
    		<div class="content">
    			<div id="data">
            <h2 id="status"></h2>
            <h3 id="postId"></h3>
						<h2>¡Llega directo por una cheve doble!</h2>
      			<p id="p-left" >¿Ya tienes tu boleto de Thermo?</p>
      			<p id="p-left" ><strong>¡Gánate una cerveza GRATIS!</strong></p>
      			<P id="p-left" class="border-bot">Lo unico que tienes que hacer es compartir esta publicación
      			   en tu Facebook para que tus amigos se enteren del evento.</P>
    			</div>
					<label>Ingresa el folio de tu ticket:</label><br>
    			<input v-model="idTicket" class="folioId" v-if="!exist"><br>
    			<button id="validate" v-if="hidefalse" @click="confirm" class="white greenBack btn">CONFIRMAR ID</button>
					<div class="green ticketValidado" v-if="exist">
						<strong>¡Boleto válido!</strong>
					</div>
    			<transition name="enterUp">
						<h2 v-if="falso" class="colorado">Id incorrecto</h2>
					</transition>
					<button id="share" v-if="exist" @click="dataPost" class="white blueBack btn">SHARE</button>
    		 <!--shareamos un post y obtenemos su id-->
    		</div>
  		</div>
	</div>
</template>

<script>
var firebase= require("firebase")

// Initialize Firebase
  var config = {
    apiKey: "AIzaSyD2i935IRjEcCXK4pw-Aqq3M19BkJ84qNg",
    authDomain: "share-25253.firebaseapp.com",
    databaseURL: "https://share-25253.firebaseio.com",
    projectId: "share-25253",
    storageBucket: "share-25253.appspot.com",
    messagingSenderId: "29176409500"
  };
  firebase.initializeApp(config);

export default {
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      idTicket: '',
      hidefalse: 'true',
      idUser: '',
      postId: '',
      exist: false,
      value: '',
      dbUser: '',
      dbId: '',
      idPost: '',
      falso: false
    }
  },
  computed: {},
  mounted () {

    var vm = this;
    // This is called with the results from from FB.getLoginStatus().
  function statusChangeCallback(response) {
    console.log('statusChangeCallback');
    console.log(response);
    // The response object is returned with a status field that lets the
    // app know the current login status of the person.
    // Full docs on the response object can be found in the documentation
    // for FB.getLoginStatus().
    if (response.status === 'connected') {
      // Logged into your app and Facebook.
      testAPI();
    } else {
      // The person is not logged into your app or we are unable to tell.
      document.getElementById('status').innerHTML = 'Please log ' +
        'into this app.';
    }
  }

  // This function is called when someone finishes with the Login
  // Button.  See the onlogin handler attached to it in the sample
  // code below.
  function checkLoginState() {
    FB.getLoginStatus(function(response) {
      statusChangeCallback(response);
    });
  }



  window.fbAsyncInit = function() {
  FB.init({
    appId      : '771510193011152',
    cookie     : true,  // enable cookies to allow the server to access
                        // the session
    xfbml      : true,  // parse social plugins on this page
    version    : 'v2.9' // use graph api version 2.8
  });

  // Now that we've initialized the JavaScript SDK, we call
  // FB.getLoginStatus().  This function gets the state of the
  // person visiting this page and can return one of three states to
  // the callback you provide.  They can be:
  //
  // 1. Logged into your app ('connected')
  // 2. Logged into Facebook, but not your app ('not_authorized')
  // 3. Not logged into Facebook and can't tell if they are logged into
  //    your app or not.
  //
  // These three cases are handled in the callback function.

  FB.getLoginStatus(function(response) {
    statusChangeCallback(response);
  });

  };

  // Load the SDK asynchronously
  (function(d, s, id) {
    var js, fjs = d.getElementsByTagName(s)[0];
    if (d.getElementById(id)) return;
    js = d.createElement(s); js.id = id;
    js.src = "//connect.facebook.net/en_US/sdk.js";
    fjs.parentNode.insertBefore(js, fjs);
  }(document, 'script', 'facebook-jssdk'));

  // Here we run a very simple test of the Graph API after login is
  // successful.  See statusChangeCallback() for when this call is made.
  // aqui incertamos todas las consultas y funciones que deseamos hacer de la api de facebook
  function testAPI() {
    console.log('Welcome!  Fetching your information.... ');
    FB.api('/me', function(response) {
      console.log('Successful login for: ' + response.name);
      document.getElementById('status').innerHTML =
        'Thanks for logging in, ' + response.name + '!';
    });

    FB.api('/me/permissions', function (response) {
      console.log(response);
    } );

    FB.api(
      '/me',
      'GET',
      {"fields":"id,name,relationship_status"},
      function(response) {
          console.log(response),
          vm.idUser = "/"+response.id;
          console.log("este es el id del usuario actual " + vm.idUser)
          //firebase.database().ref("face/").push(response);
          firebase.database().ref("/face").once('value').then(function(snap){

            console.log(snap.val()),
            vm.dbUser = snap.val();
          })
      }
    );

    // //subimos onformacion al firebase
          // submit(){
          //   var vm = this;
          //   firebase.database().ref("IdPosts/").push(vm.postId)
          // },

//ingresa aqui todo las llamadas a la api
}
  },
  methods: {
    //Compartimos un post y obtenemos su id
    //a continuacion, comprobamos que el post este en publico
    //si lo esta accedemos a ingresar el id      ¡¡----SOLO PODEMOS OBTENER EL ID DEL SHARE----!!

    dataPost(){
      var u = '/10211600055269848', // esto está hard coded
          p = '_10211657971757724',
          vm = this;
      FB.ui({
        method: 'share',
        display: 'popup',
        href: 'https://www.facebook.com/PalNorteOficial/photos/a.187269798071689.44965.185641444901191/907621902703138/?type=3&theater',
      } , function(response){
	          console.log('id Share: ');
	          if (response) {
	            console.log(response),
	            vm.idPost = response,
	            document.getElementById('postId').innerHTML = "Id del post ue acabas de publicar" + vm.idPost.post_id;
	            FB.api(
	            vm.idUser + '_'+vm.idPost.post_id,
	            'GET',
	            {"fields":"privacy,id,admin_creator,application,created_time,from,is_hidden,message,permalink_url"}, //AQUI OBTENGO TODA LA INFORMACION DEL POST
	            function(response) {
		            console.log("Datos despues de obtener el Post ID "),
		            console.log(response);
		            vm.value = response.privacy.value;
		            if (vm.value == "ALL_FRIENDS") {
		              alert("GRACIAS");

		            }else{
		              alert("ponlo como amigos");
		            }
	          	}
	        	)
          /*var vm = this;
          firebase.database().ref("IdPosts/").push(response)*/
        }
      });
    },

    //confirmamos que exista el  dato ingresado en el FIREBASE   ¡¡-- En este caso es un ID en especifico--!!
    confirm(){
        var vm = this;
        this.msg
        firebase.database().ref("/Users/").orderByValue/*oredena el valor */().equalTo/*equalto*/(vm.idTicket) //hacemos un consulta de un valor en especifico
        .once('value',function(snap) {
          console.log(snap.val());
          if( snap.val()  != null) {
            vm.exist= true;
            vm.falso=false;
            vm.hidefalse = false;
          } else{
            vm.falso=true;
          }
        });
         // firebase.database().ref("/Users").once("value").then(function(data){
        //  data = data.val();//obtener los visibles
        //  console.log(data);
        // })
        //accedemos a los datos de firebase, .ref: (hacemos   referencia a que parte queremos entrar de la base de datos)
			},
    exit() {}
  },

  components: {}

}
</script>

<style lang="css">
#data{
	;
}

#p-left{
	text-align: left;
	margin-left: 4%;
}

.btn{
	margin-top: 2%;
	width: 100%;
	background-color: #d1d1d1;
	border: 1px solid #e1e1e1;
	line-height: 80px;
	font-size: 20px;
	opacity: 0.9;
	transition: linear opacity 0.2s;
}

.btn:hover {
	cursor: pointer;
	opacity: 1;
}

.folioId{
	margin-top: 3%;
	height: 80px;
  width: 96%;
	font-size: 20px;
	padding-left: 5px;
	padding-right: 5px;
	text-align: center;
}

.ticketValidado {
	width: 100%;
	line-height: 80px;
	text-align: center;
}

#margin-bot{
	margin-top: 7%;
	border: 3px solid red;
}

.border-bot{
	margin-bottom: 4%;
}

.mainContainer {
  position: relative;
  width: 1200px;
  margin: 0 auto;
  overflow: auto;
}

.subContainer {
  position: relative;
  width: 1080px;
  margin-left: 60px;
  overflow: auto;
}

.content {
  float: left;
  width: 50%;
}

.poster {
  width: 60%;
}

.blueBack {
	background-color: #106989;
}

.greenBack {
	background-color: #45ad6e;
}

.green {
	color: #45ad6e;
}

.colorado {
	color: #ce4c4c;
}

.white {
	color: #fff;
}

@media only screen and (max-width: 1200px) {

  .mainContainer {
    width: 960px;
  }

  .subContainer {
    width: 864px;
    margin-left: 48px
  }

}

@media only screen and (max-width: 960px) {

  .mainContainer {
    width: 720px;
  }

  .subContainer {
    width: 648px;
    margin-left: 36px;
  }

  .content {
    width: 80%;
    margin-left: 10%;
    float: none;
  }

}

@media only screen and (max-width: 720px) {

  .mainContainer {
    width: 100%;
		/*
		margin-top: N px; <-- lo que sea que mida el haeder.
		*/
  }

  .subContainer {
    width: 90%;
    margin-left: 5%;
  }

  .content {
    width: 100%;
    margin-left: 0;
  }

  .poster {
    width: 100%;
    margin-left: 0;
  }

	.btn {
		margin-bottom: 50px;
	}

}
</style>
