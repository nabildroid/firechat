<template>
	<div id="sidepanel">
	  <div id="profile" :class="{expanded:dropdown.links}">
	    <div class="wrap">

	    	<div id="info" :class="{pro:profile.pro==true}">
		      <img id="profile-img" :src="profile.picture" :class="profile.status" @click="dropdown.status=!dropdown.status"/>
		      <p>{{profile.name}}</p>
		      <i class="fa fa-chevron-down expand-button" aria-hidden="true" @click="dropdown.links=!dropdown.links"></i>
		    </div>

	      <div id="status-options" :class="{active:dropdown.status}" @mouseleave="dropdown.status=0">
	        <ul>
	        	<li v-for="(statu,i) in status" :key="i" :class="{active:statu==profile.status}"
	        	 :id="'status-'+statu" @click="statuschange(statu)">
	        		<p>{{statu}}</p><span class="status-circle"></span>
	        	</li>
	        </ul>
	      </div>

	      <div id="expanded" v-if="profile.links">
	      	<template v-for="(link,i) in links">
		      	<label :for="link">
		      		<i :class="['fa', 'fa-'+link ,'fa-fw']" aria-hidden="true"></i>
		      	</label>
		      	<input :name="link" type="text" :value="profile.links[link]" @change="linkchange(link)" />	
	      	</template>
	        
	      </div>
	    </div>
	  </div>
	  <div id="search">
	    <label for=""><i class="fa fa-search" aria-hidden="true"></i></label>
	    <input type="text" v-model="search" placeholder="Search contacts..." />
	  </div>

	  <contact :search="search" :friends="friends" :selected="selected" @select="select($event)"/>

	  <div id="bottom-bar">
	  	<router-link to="/profile/friends" tag="button" id="addcontact">
	  		<i class="fa fa-user-plus fa-fw" aria-hidden="true"></i> <span>friends</span>
	  	</router-link>
	  	<router-link to="/profile/setting" tag="button" id="settings">
	  		<i class="fa fa-cog fa-fw" aria-hidden="true"></i> <span>Settings</span>
	  	</router-link>
	  </div>
	</div>
</template>

<script type="text/javascript" >
	import toprofile from "../../bus/toprofile.js";

	import contact from "./sidepanel/contact.vue";
	export default{
		name:"sidepanel",
		components:{contact},
		props:["profile","friends","selected"],
		data(){
			return {
				dropdown:{
					status:0,
					links:0
				},
				status:["online","busy","away","offline"],
				links:["facebook","twitter","instagram"],
				search:""
			}
		},
		methods:{
			linkchange(name){
				const value=event.target.value.trim();
				toprofile.$emit("update","links",[name,value]);
			},
			statuschange(value){
				console.log("side pannel trying to update status");
				toprofile.$emit("update",{status:value});
				setTimeout(()=>this.dropdown.status=0,200);
			},
			select(val){
				console.log(val);
				this.$emit("select",val);
			}
		}
	}
</script>

<style type="text/css">
	#sidepanel {
	  float: left;
	  min-width: 280px;
	  max-width: 340px;
	  width: 40%;
	  height: 100%;
	  background: #2c3e50;
	  color: #f5f5f5;
	  overflow: hidden;
	  position: relative;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel {
	    width: 58px;
	    min-width: 58px;
	  }
	}
	#sidepanel #profile {
	  width: 80%;
	  margin: 25px auto;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #profile {
	    width: 100%;
	    margin: 0 auto;
	    padding: 5px 0 0 0;
	    background: #32465a;
	  }
	}
	#sidepanel #profile.expanded .wrap {
	  height: 210px;
	  line-height: initial;
	}

	#sidepanel #profile.expanded .wrap i.expand-button {
	  -moz-transform: scaleY(-1);
	  -o-transform: scaleY(-1);
	  -webkit-transform: scaleY(-1);
	  transform: scaleY(-1);
	  filter: FlipH;
	  -ms-filter: "FlipH";
	}

	#sidepanel #profile .wrap {
	  height: 60px;
	  line-height: 60px;
	  overflow: hidden;
	  -moz-transition: 0.3s height ease;
	  -o-transition: 0.3s height ease;
	  -webkit-transition: 0.3s height ease;
	  transition: 0.3s height ease;
	}
	#sidepanel #profile .wrap #info{
		position: relative;
	}
	#sidepanel #profile .wrap #info.pro:before {
	    position: absolute;
	    content: "pro";
	    z-index: 99;
	    top: 0px;
	    left: 0;
	    line-height: 1;
	    background: #59af62;
	    font-size: 12px;
	    padding: 1px;
	    border-radius: 6px;
	}

	@media screen and (max-width: 735px) {
	  #sidepanel #profile .wrap {
	    height: 55px;
	  }
	}
	#sidepanel #profile .wrap img {
	  width: 50px;
	  border-radius: 50%;
	  padding: 3px;
	  border: 2px solid #e74c3c;
	  height: auto;
	  float: left;
	  cursor: pointer;
	  -moz-transition: 0.3s border ease;
	  -o-transition: 0.3s border ease;
	  -webkit-transition: 0.3s border ease;
	  transition: 0.3s border ease;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #profile .wrap img {
	    width: 40px;
	    margin-left: 4px;
	  }
	}
	#sidepanel #profile .wrap img.online {
	  border: 2px solid #2ecc71;
	}
	#sidepanel #profile .wrap img.away {
	  border: 2px solid #f1c40f;
	}
	#sidepanel #profile .wrap img.busy {
	  border: 2px solid #e74c3c;
	}
	#sidepanel #profile .wrap img.offline {
	  border: 2px solid #95a5a6;
	}
	#sidepanel #profile .wrap p {
	  float: left;
	  margin-left: 15px;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #profile .wrap p {
	    display: none;
	  }
	}
	#sidepanel #profile .wrap i.expand-button {
	  float: right;
	  margin-top: 23px;
	  font-size: 0.8em;
	  cursor: pointer;
	  color: #435f7a;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #profile .wrap i.expand-button {
	    display: none;
	  }
	}
	#sidepanel #profile .wrap #status-options {
	  position: absolute;
	  opacity: 0;
	  visibility: hidden;
	  width: 150px;
	  margin: 70px 0 0 0;
	  border-radius: 6px;
	  z-index: 99;
	  line-height: initial;
	  background: #435f7a;
	  -moz-transition: 0.3s all ease;
	  -o-transition: 0.3s all ease;
	  -webkit-transition: 0.3s all ease;
	  transition: 0.3s all ease;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #profile .wrap #status-options {
	    width: 58px;
	    margin-top: 57px;
	  }
	}
	#sidepanel #profile .wrap #status-options.active {
	  opacity: 1;
	  visibility: visible;
	  margin: 75px 0 0 0;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #profile .wrap #status-options.active {
	    margin-top: 62px;
	  }
	}
	#sidepanel #profile .wrap #status-options:before {
	  content: '';
	  position: absolute;
	  width: 0;
	  height: 0;
	  border-left: 6px solid transparent;
	  border-right: 6px solid transparent;
	  border-bottom: 8px solid #435f7a;
	  margin: -8px 0 0 24px;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #profile .wrap #status-options:before {
	    margin-left: 23px;
	  }
	}
	#sidepanel #profile .wrap #status-options ul {
	  overflow: hidden;
	  border-radius: 6px;
	}
	#sidepanel #profile .wrap #status-options ul li {
	  display: block;
	  cursor: pointer;
	  overflow: hidden;
	  line-height: 1.5;
	  padding-top:5px ; 
	  padding-bottom:5px ; 
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #profile .wrap #status-options ul li {
	    padding: 15px 0 35px 22px;
	  }
	}
	#sidepanel #profile .wrap #status-options ul li:hover {
	  background: #496886;
	}
	#sidepanel #profile .wrap #status-options ul li span.status-circle {
	  width: 10px;
	  height: 10px;
	  border-radius: 50%;
	  float: right;
	  margin-top: 5px;
	  margin-right: 20px;
	  display: inline-block;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #profile .wrap #status-options ul li span.status-circle {
	    width: 14px;
	    height: 14px;
	  }
	}
	#sidepanel #profile .wrap #status-options ul li span.status-circle:before {
	  content: '';
	  position: absolute;
	  width: 16px;
	  height: 16px;
	  margin: -3px 0 0 -3px;
	  background: transparent;
	  border-radius: 50%;
	  z-index: 0;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #profile .wrap #status-options ul li span.status-circle:before {
	    height: 18px;
	    width: 18px;
	  }
	}
	#sidepanel #profile .wrap #status-options ul li p {
	  padding-left: 12px;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #profile .wrap #status-options ul li p {
	    display: none;
	  }
	}
	#sidepanel #profile .wrap #status-options ul li#status-online span.status-circle {
	  background: #2ecc71;
	}
	#sidepanel #profile .wrap #status-options ul li#status-online.active span.status-circle:before {
	  border: 1px solid #2ecc71;
	}
	#sidepanel #profile .wrap #status-options ul li#status-away span.status-circle {
	  background: #f1c40f;
	}
	#sidepanel #profile .wrap #status-options ul li#status-away.active span.status-circle:before {
	  border: 1px solid #f1c40f;
	}
	#sidepanel #profile .wrap #status-options ul li#status-busy span.status-circle {
	  background: #e74c3c;
	}
	#sidepanel #profile .wrap #status-options ul li#status-busy.active span.status-circle:before {
	  border: 1px solid #e74c3c;
	}
	#sidepanel #profile .wrap #status-options ul li#status-offline span.status-circle {
	  background: #95a5a6;
	}
	#sidepanel #profile .wrap #status-options ul li#status-offline.active span.status-circle:before {
	  border: 1px solid #95a5a6;
	}
	#sidepanel #profile .wrap #expanded {
	  padding: 100px 0 0 0;
	  display: block;
	  line-height: initial !important;
	}
	#sidepanel #profile .wrap #expanded label {
	  float: left;
	  clear: both;
	  margin: 0 8px 5px 0;
	  padding: 5px 0;
	}
	#sidepanel #profile .wrap #expanded input {
	  border: none;
	  margin-bottom: 6px;
	  background: #32465a;
	  border-radius: 3px;
	  color: #f5f5f5;
	  padding: 7px;
	  width: calc(100% - 43px);
	}
	#sidepanel #profile .wrap #expanded input:focus {
	  outline: none;
	  background: #435f7a;
	}
	#sidepanel #search {
	  border-top: 1px solid #32465a;
	  border-bottom: 1px solid #32465a;
	  font-weight: 300;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #search {
	    display: none;
	  }
	}
	#sidepanel #search label {
	  position: absolute;
	  margin: 10px 0 0 20px;
	}
	#sidepanel #search input {
	  font-family: "proxima-nova",  "Source Sans Pro", sans-serif;
	  padding: 10px 0 10px 46px;
	  width: calc(100% - 25px);
	  border: none;
	  background: #32465a;
	  color: #f5f5f5;
	}
	#sidepanel #search input:focus {
	  outline: none;
	  background: #435f7a;
	}
	#sidepanel #search input::-webkit-input-placeholder {
	  color: #f5f5f5;
	}
	#sidepanel #search input::-moz-placeholder {
	  color: #f5f5f5;
	}
	#sidepanel #search input:-ms-input-placeholder {
	  color: #f5f5f5;
	}
	#sidepanel #search input:-moz-placeholder {
	  color: #f5f5f5;
	}
	#sidepanel #contacts {
	  height: calc(100% - 177px);
	  overflow-y: scroll;
	  overflow-x: hidden;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #contacts {
	    height: calc(100% - 149px);
	    overflow-y: scroll;
	    overflow-x: hidden;
	  }
	  #sidepanel #contacts::-webkit-scrollbar {
	    display: none;
	  }
	}
	#sidepanel #contacts.expanded {
	  height: calc(100% - 334px);
	}
	#sidepanel #contacts::-webkit-scrollbar {
	  width: 8px;
	  background: #2c3e50;
	}
	#sidepanel #contacts::-webkit-scrollbar-thumb {
	  background-color: #243140;
	}
	#sidepanel #contacts ul li.contact {
	  position: relative;
	  padding: 10px 0 15px 0;
	  font-size: 0.9em;
	  cursor: pointer;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #contacts ul li.contact {
	    padding: 6px 0 46px 8px;
	  }
	}
	#sidepanel #contacts ul li.contact:hover {
	  background: #32465a;
	}
	#sidepanel #contacts ul li.contact.active {
	  background: #32465a;
	  border-right: 5px solid #435f7a;
	}
	#sidepanel #contacts ul li.contact.active span.contact-status {
	  border: 2px solid #32465a !important;
	}
	#sidepanel #contacts ul li.contact .wrap {
	  width: 88%;
	  margin: 0 auto;
	  position: relative;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #contacts ul li.contact .wrap {
	    width: 100%;
	  }
	}
	#sidepanel #contacts ul li.contact .wrap span {
	  position: absolute;
	  left: 0;
	  margin: -2px 0 0 -2px;
	  width: 10px;
	  height: 10px;
	  border-radius: 50%;
	  border: 2px solid #2c3e50;
	  background: #95a5a6;
	}
	#sidepanel #contacts ul li.contact .wrap span.online {
	  background: #2ecc71;
	}
	#sidepanel #contacts ul li.contact .wrap span.away {
	  background: #f1c40f;
	}
	#sidepanel #contacts ul li.contact .wrap span.busy {
	  background: #e74c3c;
	}
	#sidepanel #contacts ul li.contact .wrap img {
	  width: 40px;
	  border-radius: 50%;
	  float: left;
	  margin-right: 10px;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #contacts ul li.contact .wrap img {
	    margin-right: 0px;
	  }
	}
	#sidepanel #contacts ul li.contact .wrap .meta {
	  padding: 5px 0 0 0;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #contacts ul li.contact .wrap .meta {
	    display: none;
	  }
	}
	#sidepanel #contacts ul li.contact .wrap .meta .name {
	  font-weight: 600;
	}
	#sidepanel #contacts ul li.contact .wrap .meta .preview {
	  margin: 5px 0 0 0;
	  padding: 0 0 1px;
	  font-weight: 400;
	  white-space: nowrap;
	  overflow: hidden;
	  text-overflow: ellipsis;
	  -moz-transition: 1s all ease;
	  -o-transition: 1s all ease;
	  -webkit-transition: 1s all ease;
	  transition: 1s all ease;
	}
	#sidepanel #contacts ul li.contact .wrap .meta .preview span {
	  position: initial;
	  border-radius: initial;
	  background: none;
	  border: none;
	  padding: 0 2px 0 0;
	  margin: 0 0 0 1px;
	  opacity: .5;
	}
	#sidepanel #bottom-bar {
	  position: absolute;
	  width: 100%;
	  bottom: 0;
	}
	#sidepanel #bottom-bar button {
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
	@media screen and (max-width: 735px) {
	  #sidepanel #bottom-bar button {
	    float: none;
	    width: 100%;
	    padding: 15px 0;
	  }
	}
	#sidepanel #bottom-bar button:focus {
	  outline: none;
	}
	#sidepanel #bottom-bar button:nth-child(1) {
	  border-right: 1px solid #2c3e50;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #bottom-bar button:nth-child(1) {
	    border-right: none;
	    border-bottom: 1px solid #2c3e50;
	  }
	}
	#sidepanel #bottom-bar button:hover {
	  background: #435f7a;
	}
	#sidepanel #bottom-bar button i {
	  margin-right: 3px;
	  font-size: 1em;
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #bottom-bar button i {
	    font-size: 1.3em;
	  }
	}
	@media screen and (max-width: 735px) {
	  #sidepanel #bottom-bar button span {
	    display: none;
	  }
	}
</style>