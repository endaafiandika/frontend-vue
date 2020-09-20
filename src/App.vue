<template>
  <div id="app">
    <div class="row">
    <div class="col-md-2">
      <center>
				<div class="card">
					<div class="card-header" id="head"><a href='' title='Add'><img src='https://image.flaticon.com/icons/svg/54/54878.svg' width='55'/></a><br><br></div>
					<div class="card-body">
						<b-img src="https://previews.123rf.com/images/rrraven/rrraven1709/rrraven170900039/86130765-spoon-fork-and-knife-vector-.jpg" width="65" v-b-modal.modal-1></b-img><br><br>
						<router-link to="/history">
            <b-img src="https://cdn.icon-icons.com/icons2/1522/PNG/512/taskboardlinear_106203.png" 	width="65"></b-img>
        </router-link>
							<b-modal id="modal-1" title="Add" hide-footer>
								<form>
								<b-form-group 
								label="Nama"
								>
								<b-form-input
								id="name-input"
								v-model="form.nama">
								</b-form-input>
								</b-form-group>
								<b-form-group 
								label="Harga"
								>
								<b-form-input
								id="name-input"
								v-model="form.harga">
								</b-form-input>
								</b-form-group>
								<b-form-group 
								label="Stok"
								>
								<b-form-input
								id="name-input"
								v-model="form.stok">
								</b-form-input>
								</b-form-group>
								<b-form-group 
								label="Url Images"
								>
								<b-form-input
								id="name-input"
								v-model="form.images">
								</b-form-input>
								</b-form-group>		
								<button class="btn btn-primary" @click="addProduct()">Save</button>
								</form>
              </b-modal>
			<b-modal id="modal-edit" title="Edit Produk" hide-footer>
								<form>
								<b-form-group 
								label="enter id"
								>
								<b-form-input
								id="name-input"
								v-model="form.id">
								</b-form-input>
								</b-form-group>
								<b-form-group 
								label="enter nama"
								>
								<b-form-input
								id="name-input"
								v-model="form.nama">
								</b-form-input>
								</b-form-group>
								<b-form-group 
								label="enter harga"
								>
								<b-form-input
								id="name-input"
								v-model="form.harga">
								</b-form-input>
								</b-form-group>
								<b-form-group 
								label="enter stok"
								>
								<b-form-input
								id="name-input"
								v-model="form.stok">
								</b-form-input>
								</b-form-group>
								<b-form-group 
								label="enter images"
								>
								<b-form-input
								id="name-input"
								v-model="form.images">
								</b-form-input>
								</b-form-group>		
								<button class="btn btn-primary" @click="edit()">Save</button>
								</form>
              </b-modal>
			<b-modal id="modal-delete" title="Delete Produk" hide-footer>
								<form>
								<b-form-group 
								label="ID"
								>
								<b-form-input
								id="name-input"
								v-model="form.id">
								</b-form-input>
								</b-form-group>	
								<button class="btn btn-primary" @click="del(id)">Delete</button>
								</form>
              </b-modal>
	
					</div>
				</div>
			</center>
    </div>
		<div class="col-md-7">
			<div class="card" id="layoutbox1">
				<div class="card-header">
					<div class="col-md-12">
            <b-nav-form>
              <b-form-input size="sm" class="mr-sm-2" placeholder="Search"></b-form-input>
              <b-button size="sm" class="my-2 my-sm-0" type="submit">Search</b-button>
				<b-nav-item-dropdown text="" right>
                <b-dropdown-item to="/nama">Nama</b-dropdown-item>
              </b-nav-item-dropdown>
            </b-nav-form>
						<h5 style="font-family: cursive; color: black; text-align: center;">Food Item</h5>
					</div>
				</div>
				<div class="container">
				<div class="card" v-for="item in data" :key="item.id">
						<b-img :src=item.images :alt="item" width="200" @click="addCart(item)"></b-img>
						<h2><center>{{ item.nama }}</center></h2>	
						<p><center>Harga :Rp {{ item.harga }}</center></p> 
				<b-button variant="success" v-b-modal.modal-edit>Edit</b-button>
                <b-button variant="primary" v-b-modal.modal-delete>Delete</b-button>
				<br>   
				</div>
				</div>
			</div>
		</div>

    <div class="col-md-3">
			<div class="card" id="layoutbox2">
				<div class="card-header" id="head2">
					<h2 align="center">Cart {{cart.length}}</h2>
				</div>
				<div class="card-body">
					<div class="card" v-for="item in cart" :key="item.id">
						<b-img :src=item.images :alt="item" width="100" @click="addCart(item)"></b-img>
						<h2><center> {{ item.nama }}</center></h2>	
						<p><center>Harga :Rp {{ item.harga }}</center></p> 
						
				</div>
					<div class="but">
						<h5 style="float: left; margin-right: 20px;"></h5>
						<h6 style="float: right;"></h6>
						<h5 style="float: left;"></h5>
						<button type="button" class="btn btn-danger btn-block" data-toggle="modal" data-target="#co">Checkout</button>
						<button class="btn btn-success btn-block">Cancel</button>
					</div>
				</div>
			</div>

      
		</div>
    </div>
  </div>
</template>

<script>
import axios from "axios"

export default {
  name: 'App',
  data() {
	return {
		form : {
			id:'',
			nama:'',
			harga:'',
			stok:'',
			images:''
		},
		
	data : [],
	cart : []
	}
	},

	async mounted(){
		this.load()
	},
	
	methods : {
		async load() {
		const response = await axios.get(process.env.VUE_APP_URL)
		this.data = response.data
	},	

	addCart(data) {
		this.cart.push(data)
	},

	addProduct(){
		try {
		const response = axios.post(process.env.VUE_APP_URL, this.form)
		this.data = response.data
		} catch (error) {
			console.log(error)
		}
	},
	edit(){
		try {
		const response = axios.put(process.env.VUE_APP_URL, this.form)
		this.data = response.data
		} catch (error) {
			console.log(error)
		}
	},
	del(){
		try {
		const response = axios({
			method: "DELETE",
			url : process.env.VUE_APP_URL,
			data: {id : this.form.id}
		})
		this.data = response.data
		} catch (error) {
			console.log(error)
		}
	},
	search(){
      try {
        const response = axios({
          method: "GET",
          url: 'http://localhost:3000/product/search?nama={{nama}}' ,
          data: {nama : this.form.nama}
        })
        this.data = response.data  
      } catch (err) {
        console.log(err)
		} 
	}
	}
	
}


</script>

<style>
@import url(http://fonts.googleapis.com/css?family=Open+Sans:400,800,700,600,300);

body {
	background: #eee;
	font-family: 'Open Sans', sans-serif;
}

hr {
	border:0;
	background:#dedede;
	height:1px;
}		
#makanan{
	float: left;
}

#layoutbox{
	height: 750px;
	width: 250px;
}
#head{
	height: 80px;
}
#layoutbox1{
	width: 800px;
	height: 750px;
}
#layoutbox2{
	margin-left: auto;
	width: 290px;
	height: 750px;
}
#head2{
	height: 85px;
}
#harga{
	float: right;
}
/*sidebar bagian kiri*/
.sidebar1 {
	float:left;
	width:175px;
	margin-right: 5px;
}
/*sidebar bagian kiri*/
.sidebar1 .widget1 {
	padding:25px;
	height: 600px;
	background:#fff;
	border-bottom: 2px solid #fff;
	transition: all 0.5s ease;
}
.sidebar1 .widget1:hover {
	border-bottom: 2px solid #3498db;
}

.sidebar1 .widget1 h2 {
	padding:3px;
	margin:0 0 15px;
	color:#3498db;
	font-size: 18px;
	font-weight:800;
	text-transform: uppercase;
}
.container{
	display: flex;
	flex-flow: row wrap;
	justify-content: space-around;
}

.sidebar1 .widget1 p {
	font-size: 14px;
}

.sidebar1 .widget1 p:last-child {
	margin:0;
}

.wrap {
	width: 100%;

}
/*pembuka sidebar2*/
.sidebar2 {
	float:left;
	width:158px;
	margin-right: 5px;
}
.sidebar2 .widget2 {
	padding:25px;
	height: 600px;
	background:#fff;
	border-bottom: 2px solid #fff;
	transition: all 0.5s ease;
}
.sidebar2 .widget2:hover {
	border-bottom: 2px solid #3498db;
}
#h6tulisan{
	float: left;
	margin-left :10px;
}
#h6harga{
	float: right;
	margin-top: 35px;
}
.but{
	margin-top: 450px;
}
.sidebar2 .widget2 h2 {
	padding:3px;
	margin:0 0 15px;
	color:#3498db;
	font-size: 18px;
	font-weight:800;
	text-transform: uppercase;
}

.sidebar2 .widget2 p {
	font-size: 14px;
}

.sidebar2 .widget2 p:last-child {
	margin:0;
}
/*penutup sidebar2*/
/*box1*/
.box1{
	text-align: center;
	padding-top: 25px;	
	color: white;
	width: 200px;
	background: red;
	height: 60px;
	box-shadow: 0 10px 13px gray;
}
.box2{
	float: right;
	text-align: center;
	padding-top: 25px;	
	color: white;
	width: 200px;
	background: red;
	height: 60px;
	box-shadow: 0 10px 13px gray;
}
/*untuk sidebar bagian kanan*/
.sidebar {
	float: right;
	width: 160px;
	margin-right: 100px;
}

.sidebar .widget {
	padding:25px;
	margin:0 0 25px;
	width: 200px;
	height: 600px;
	background:#fff;
	border-bottom: 2px solid #fff;
	transition: all 0.5s ease;
}
.sidebar .widget img{
	float: left;
	margin-top: 20px;
	height: 60px;
}
.co{
	padding: 0px;
}
.sidebar .widget h3{
	float: right;
	margin-right: 30px;
}
.sidebar .widget h5{
	float: right;
	margin-left: 45px;
}
#ha3{
	float: right;
}
.sidebar .widget button{
	background: green;
	width: 100%;
	height: 30px;
}
#btn{
	background: red;
	color: white;
	width: 100%;
	height: 30px;
}
.box-min{
	float: left;
	display: flex;
	flex-wrap: wrap;
	background: white;
	color: white;
}

.box-min > div{
	float: left;
	background-color: green;
	width: 20px;
	margin :5px;
	text-align: center;
	height: 25px;
	box-shadow: 0 5px 10px gray;
}
.sidebar .widget:hover {
	border-bottom: 2px solid #3498db;
}

.sidebar .widget h2 {
	padding:0;
	margin:0 0 15px;
	color:#3498db;
	font-size: 18px;
	font-weight:800;
	text-transform: uppercase;
}
/*untuk blog di gunakan untuk mengatur blog*/
.blog {
	margin-right: 90px;

}

.conteudo {
	width:800px;
	padding:25px;
	height: 600px;
	margin:20px auto;
	background: #fff;
	border:1px solid #dedede;
	-webkit-box-shadow: 1px 1px 1px 0px rgba(204,204,204,0.35);
	-moz-box-shadow: 1px 1px 1px 0px rgba(204,204,204,0.35);
	box-shadow: 1px 1px 1px 0px rgba(204,204,204,0.35);
}

.conteudo img {
	float: right;
	width: 30px;
}
.conten {
	width:1100px;
	padding:25px;
	height: 600px;
	margin-left: 160px;
	background: #fff;
	border:1px solid #dedede;
	-webkit-box-shadow: 1px 1px 1px 0px rgba(204,204,204,0.35);
	-moz-box-shadow: 1px 1px 1px 0px rgba(204,204,204,0.35);
	box-shadow: 1px 1px 1px 0px rgba(204,204,204,0.35);
}

.conten img {
	float: right;
	width: 30px;
}
.pinggir{
	height: 500px;
}
#delap{
	margin-right: 90px;
}
.conteudo h2 {
	padding:0;
	text-align: center;
	margin:0 0 15px;
	font-weight: normal;
	color: #666;
	font-family: Georgia;
}

.conteudo p:last-child {
	margin: 0;
}

.conteudo .continue-lendo {
	color:#000;
	transition: all 0.5s ease;
	text-decoration: none;
	font-weight: 700; 
}

.conteudo .continue-lendo:hover {
	margin-left:10px;
}

.conten h2 {
	padding:0;
	text-align: center;
	margin:0 0 15px;
	font-weight: normal;
	color: #666;
	font-family: Georgia;
}

.conten p:last-child {
	margin: 0;
}

.conten .continue-lendo {
	color:#000;
	transition: all 0.5s ease;
	text-decoration: none;
	font-weight: 700; 
}

.conten .continue-lendo:hover {
	margin-left:10px;
}
.post-info {
	float: right;
	font-size: 12px;
	margin: -10px 0 15px;
	text-transform: uppercase;
}

@media screen and (max-width: 960px) {

	.header {
		position:inherit;
	}

	.wrap {
		width: 90%;
		margin:25px auto;
	}
	/*.sidebar1 {
		width:100%;
		margin:25px 0 0;
		float:right;
	}
 
	.sidebar1 .widget1 {
		padding:5%;
	}
	.sidebar {
		width:100%;
		margin:25px 0 0;
		float:right;
	}
 
	.sidebar .widget {
		padding:5%;
		}*/

		nav.menu ul {
			width: 100%;
		}

		nav.menu ul {
			float:inherit;
		}

		nav.menu ul li {
			float:inherit;
			margin:0;
		}

		nav.menu ul a {
			padding:15px;
			font-size: 16px;
			border-top:1px solid #1abf9f;
			border-bottom:1px solid #16a085;
		}

		.blog {
			width:90%;
		}

		.conteudo {
			float:inherit;
			margin:0 auto 25px;
			width:101%;
			border:1px solid #dedede;
			/*padding:5%;  */
			background: #fff;
		}

		.conteudo img {
			max-width: 110%;
			margin:0 0 25px -5%;
			min-width: 110%;
		}

		.conteudo .continue-lendo:hover {
			margin-left:0;
		}

		.conten {
			float:inherit;
			margin:0 auto 25px;
			width:101%;
			border:1px solid #dedede;
			/*padding:5%;  */
			background: #fff;
		}

		.conten img {
			max-width: 110%;
			margin:0 0 25px -5%;
			min-width: 110%;
		}

		.conten .continue-lendo:hover {
			margin-left:0;
		}
	}

	@media screen and (max-width: 460px) {

		nav.menu ul a {
			padding:15px;
			font-size: 14px;
		}

	/*.sidebar {
		display:none
	}
	.sidebar1 {
		display:none
		}*/
		.post-info {
			display:none;
		}

	/*.conteudo {
		margin:25px auto;
		}*/

		.conteudo img {
			margin:-5% 0 25px -5%;
		}
		.conten img {
			margin:-5% 0 25px -5%;
		}
		
	}
	
	.flex-container {
		display: flex;
		flex-wrap: wrap;
		background-color: #f1efef;
	}

	.flex-container > div {
		background-color: #f1f1f1;
		width: 100px;
		margin: 5px;
		text-align: center;
		line-height: 75px;
		font-size: 30px;
	}			
	.flex-container img{
		width: 100px;
		padding-left: 5px;
	}
	.flex-container h1{
		font-size: 10px;
		padding: 3px;
	}
</style>
