<template>
	<div id="setting">
		<header>
			<img :src="profile.picture">
			<b v-text="profile.name"></b>
		</header>
		<div id="sections">
			<div id="tool">
				<button v-for="btn in sections" @click="section=btn" :class="{active:section==btn}">{{btn}}</button>
			</div>
			<component :is="section" v-bind="{profile:profile}"/>
		</div>
		<div id="bottom-bar">
			<router-link to="profile" tag="button" id="addcontact">
				<i class="fa fa-cog fa-fw" aria-hidden="true"></i> <span>Messages</span>
			</router-link>
			<router-link to="/profile/friends" tag="button" id="settings">
				<i class="fa fa-user-plus fa-fw" aria-hidden="true"></i> <span>friends</span>
			</router-link>
		</div>
	</div>
</template>

<script type="text/javascript">
	import info from "../../components/setting/info";	
	import password from "../../components/setting/password";	
	export default{
		name:"setting",
		components:{info,password},
		props:["profile"],
		data(){
			return {
				sections:["info","password"],
				section:"info"
			}	
		},
		created(){
			console.log("setting created");
		}
	}
</script>



<style type="text/css" scoped="">
	
		div#setting {
		    height: 100%;
		    background: #2c3e50;
		    color: #f5f5f5;
		    overflow-y: auto;
		    overflow-x: hidden;
		    position: relative;
		    padding: 20px 70px;

		}
		div#setting::-webkit-scrollbar {
		  width: 8px;
		  background: #2c3e50;
		}
		div#setting::-webkit-scrollbar-thumb {
		  background-color: #243140;
		}
		div#setting header {
		    width: 100%;
		    height: 64px;
		    overflow: hidden;
		    line-height: 50px;
		    border-bottom: 1px solid #425b73;
		    margin-bottom: 2em;
		}
		div#setting header img {
		    width: 48px;
		    margin: 1px;
		    border-radius: 50%;
		    vertical-align: middle;
		}
		div#setting header b {
		    margin-left: 15px;
		    font-size: 24px;
		    vertical-align: middle;
		}
		div#setting #sections >>>  div#tool {
		    width: 100%;
		    height: 30px;
		    overflow: hidden;
		    line-height: 30px;
		    margin-bottom: 1em;
		}
		div#setting #sections > >>> div#tool {
	    border-bottom: 1px solid #33475a;
	  }

		div#setting >>> div#tool button {
		    background: #33475a;
		    color: #fff;
		    border: none;
		    font-size: 15px;
		    min-width: 60px;
		    padding: 0 20px;
		    position: relative;
		    transition: 0.3s;
		}

		div#setting #sections > >>> div#tool button:not(:last-child):before {content: "";position: absolute;right: 0;height: 70%;top: 15%;border-right: 1px solid #425b73;}

		div#setting #sections > >>> div#tool button.active {
		    background: #425d77;
		}

		div#setting >>> div#tool button:hover {
		    background: #425c77;
		}

		div#sections {
		    min-height: calc( 100% - 64px - 2em - 34px );
		}
		>>>  div#inputs > div {
		    display: inline-block;
		    width: 50%;
		    vertical-align: middle;
		}

		>>> div#inputs #picture {
		    position: relative;
		    width: 220px;
		    height: 220px;
		    overflow: hidden;
		    margin-right: calc( 50% - 220px );
		    box-shadow: 1px 1px 5px 1px #29292991;
		    border-radius: 5px;
		}

		>>> div#inputs {overflow: hidden;padding: 1em;}

		>>> div#inputs #picture input {
		    position: absolute;
		    top: 0;
		    left: 0;
		    width: 100%;
		    height: 100%;
		    cursor: pointer;
		    z-index: 9999;
		    opacity: 0;
		}

		>>> div#inputs #picture input img {
		    z-index: -1;
		}

		>>> div#inputs #picture img {
		    width: 100%;
		    height: 100%;
		    transition: 0.3s;
		}

		>>> div#inputs #picture:hover img {
		    transform: rotate(7deg) scale(1.1);
		}

		>>> div#inputs #picture span {
		    position: absolute;
		    top: 20%;
		    left: 25%;
		    font-size: 28px;
		    text-align: center;
		    padding: 6px 12px;
		    border: 1px solid;
		    color: #ffffff;
		    border-radius: 3px;
		    background: #33475a5e;
		    cursor: pointer;
		    transition: 0.3s;
		}

		>>> div#inputs #picture:hover span {
		    top: 30%;
		    font-size: 31px;
		    left: 27%;
		}

		>>> div#inputs >div >div input {
		}

		>>> div#inputs div >div input {
		    display: block;
		    width: 100%;
		    background: transparent;
		    border: none;
		    color: #fff;
		    font-size: 17px;
		    border-bottom: 1px solid #425c77;
		    padding: 5px 1px 10px;
		    margin-bottom: 10px;
		    outline: none;
		}

		>>> div#inputs div >div input lable {
		    color: #ddd;
		}

		>>> div#inputs div >div label {
		    color: #ddd;
		    font-size: 14px;
		    margin-bottom: 2px;
		}

		>>> div#inputs >div >div:not(:last-child) {
		    margin-bottom: 26px;
		}

		>>> div#inputs div >div input:focus {
		    border-color: #fff;
		}

	 #bottom-bar {
		  position: relative;
		  width: 209px;
		  bottom: 0;
		  overflow: hidden;
		}
		
		 #bottom-bar button {
		  float: left;
		  border: none;
		  width: 50%;
		  padding: 10px 0;
		  background: #32465a;
		  color: #f5f5f5;
		  cursor: pointer;
		  font-size: 0.85em;
		  font-family: "proxima-nova",  "Source Sans Pro", sans-serif;
		}
		 #bottom-bar button:focus {
		  outline: none;
		}
		 #bottom-bar button:nth-child(1) {
		  border-right: 1px solid #2c3e50;
		}
		
		 #bottom-bar button:hover {
		  background: #435f7a;
		}
		 #bottom-bar button i {
		  margin-right: 3px;
		  font-size: 1em;
		}

	@media screen and (max-width: 735px) {



		div#setting {
		    padding: 10px 10px;
		    padding-bottom: 2em;
		}

		div#setting header {
		    margin-bottom: 1em;
		}

		>>> div#inputs > div {
		    display: block;
		    width: 100%;
		}

		>>> div#inputs #picture {
		    margin: 0 auto;
		    margin-bottom: 3em;
		}

		#bottom-bar {
			left: 0;
		 width:100%;
		 position:fixed;
		}
		#bottom-bar button i {
		  font-size: 1.3em;
		}
	}
</style>
