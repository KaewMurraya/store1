<template>
  <div class="header">
    <div class="tabs is-toggle  is-right">
  <ul>
    <li  @click="typeOrder()">
      <a>
        <span class="icon  is-info is-small"><i class="fas fa-film" aria-hidden="true"></i></span>
        <span>Order</span>
      </a>
    </li>
    <li  @click="typestock()">
      <a>
        <span class="icon is-info is-small"><i class="far fa-file-alt" aria-hidden="true"></i></span>
        <span>Stock</span>
      </a>

    </li>
  </ul>
</div>


<div v-if = "type === 'stock'" >
<center>
<img width="10%" src="../assets/store.png">  <img width="30%" src="../assets/hedgehog.png"></center>
<div class="columns">
  <div class="column"></div>
    <div class="column is-two-thirds">
      <table>
        <tr>
          <td> <input class="input is-info is-rounded is-loading" type="text" placeholder="ชื่อสินค้า"  v-model="data.name" ></td>  &nbsp; &nbsp; &nbsp; &nbsp;
          <td> <input class="input is-info is-rounded is-loading" type="text" placeholder="จำนวน"  v-model="data.number"></td>&nbsp; &nbsp; &nbsp; &nbsp;
          <td> <input class="input is-info is-rounded is-loading" type="text" placeholder="ราคา"  v-model="data.price"></td> &nbsp; &nbsp; &nbsp; &nbsp;
          <td>
            <div class="field">
                  <label class="file-label">
                  <input class="file-input" type="file" name="resume"  @change="onFileChange($event.target.files[0])"required>
                    <span class="file-cta">
                      <span class="file-icon">
                        <i class="fas fa-cloud-upload-alt"></i>
                      </span>
                      <span class="file-label">
                        Choose a file…
                      </span>
                    </span>
                  </label>
                </div>
          </td> &nbsp; &nbsp; &nbsp; &nbsp;
          <td> <a class="button is-info is-rounded"  @click="insert()">+ADD</a> </td>
        </tr>
      </table>
    </div>
  <div class="column"></div>
</div>

<div class="columns">
  <div class="column"></div>
      <table class="table is-fullwidth" >
        <thead>
                <tr>
                      <th scope="col">Pic</th>
                      <th scope="col">Name</th>
                      <th scope="col">Number</th>
                      <th scope="col">Price</th>
                      <th scope="col">Update</th>
                      <th scope="col">Delete</th>
                  </tr>
        </thead>
        <tbody v-for =" (stock,key) in showstock"  >
          <tr  v-if = "sw !== key" >
            <td>  <img :src="stock.image"style="width: 200px; height: 100px;"></td>
            <td> </br> </br>{{stock.name}}</td>
            <td> </br></br> {{stock.number}}</td>
            <td> </br></br> {{stock.price}}</td>

            <td> </br></br><a class="button is-info is-outlined" @click="swap(key)">Update</a> </td>
            <td> </br></br><a class="button is-info is-outlined" @click="Delete(key)">Delete</a> </td>
         </tr>
            <tr  v-else >
              <td>  <input class="input is-info is-rounded" type="text"   v-model="stock.name" ></td>
              <td>   <input class="input is-info is-rounded" type="text"  v-model="stock.number" ></td>
              <td>  <input class="input is-info is-rounded" type="text"   v-model="stock.price" ></td>
              <td>
                <div class="file">
                      <label class="file-label">
                      <input class="file-input" type="file" name="resume"  @change="onFileChange($event.target.files[0])"required>  &nbsp; &nbsp; &nbsp; &nbsp;
                        <span class="file-cta">
                          <span class="file-icon">
                            <i class="fas fa-cloud-upload-alt"></i>
                          </span>
                          <span class="file-label">
                            Choose a file…
                          </span>
                        </span>
                      </label>
                    </div>
              </td>
              <td> <a class="button is-info is-outlined" @click="Update(key,stock.name,stock.number,stock.price)">Save</a> </td>
              <td> <a class="button is-info is-outlined" @click="cancel()">Cancle</a> </td>
           </tr>
            </tbody>
      </table>
    </div>
  <div class="column"></div>


</div>
<!-- จบstock -->
<div class="order"  v-if = "type === 'Order'" >
<img width="20%" src="../assets/store.png">  <img width="50%" src="../assets/hedgehog.png">
<br>

      <table class="table is-striped is-narrow is-hoverable is-fullwidth" >

        <tbody v-for =" (stock,key) in showstock"  >
          <tr>
            <td >  <img :src="stock.image"style="width: 150px; height: 200px;"></td>
            <td>  </br></br></br><b>  {{stock.name}} </b>
            </br>  จำนวน  {{stock.number}}
            </br> ราคา {{stock.price}}
            </td>
            <td> </br></br></br></br>
                <div class="select">
                  <select v-model="selected">
                    <option disabled value="">select</option>
                    <option>1</option>
                    <option>2</option>
                    <option>3</option>
                    <option>4</option>
                    <option>5</option>
                  </select>
                </div>
            </td>
            <td></td><td></td>
            <td> </br></br></br> </br>
               <!-- @click="bye(key,stock.number)" -->
                  <a  class="button is-success" @click="confirm(stock.name,stock.number,selected,stock.price,key)">
                    <span class="icon is-small">
                      <i class="cart-arrow-down"></i>
                    </span>
                    <span>BUY</span>
                  </a>
            </td>
         </tr>
            </tbody>
      </table>
</div>
</div>
</template>

<script>
import firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyAc1E7yglvcuwj44t4IJRI4-4m1Og6bKDk',
  authDomain: 'store-cdd33.firebaseapp.com',
  databaseURL: 'https://store-cdd33.firebaseio.com',
  projectId: 'store-cdd33',
  storageBucket: 'store-a7427.appspot.com',
  messagingSenderId: '356296828842'
  }
  firebase.initializeApp(config)
export default {
  name: 'HelloWorld',
  data () {
    return {
      data: {
        name: '',
        number: '',
        price: '',
        image: ''
      },
      showstock: '',
      sw: '',
      type: 'Order'
    }
  },
  created: function () {
    this.pullData()
  },
  methods: {
    pullData: function () {
      let that = this
      firebase.database().ref('/stock/').once('value').then(function (snapshot) {
        that.showstock = snapshot.val()
      })
    },
    async insert () {
      let urlsImg = await this.createImage()
      this.data.image = urlsImg.downloadURL
      firebase.database().ref('stock').push(this.data)
      this.data.name = ''
      this.data.number = ''
      this.data.price = ''
      this.data.image = ''
      this.pullData()
    },
    async Update (key, name, number, price) {
      this.sw = ''
      let urlsImg = await this.createImage()
      firebase.database().ref('/stock/').child(key).update({
        name: name,
        number: number,
        price: price,
        image: urlsImg.downloadURL
      })
      this.sw = 'update'
      this.pullData()
    },
    Delete (key) {
      firebase.database().ref('stock').child(key).remove()
      this.pullData()
    },
    swap: function (key) {
      this.sw = key
    },
    cancel () {
      this.sw = ''
    },
    onFileChange (fileImg) {
      this.dataImg = fileImg
    },
    createImage () {
      const storageRef = firebase.storage().ref('image/' + this.dataImg.name.toLowerCase().split(' ').join('-'))
      const uploadTask = storageRef.put(this.dataImg)
      return uploadTask
    },
    typestock () {
      this.type = 'stock'
    },
    typeOrder () {
      this.type = 'Order'
    },
    bye (key, number) {
      firebase.database().ref('/stock/').child(key).update({
        number: number - 1
      })
      this.pullData()
    },
    confirm(name,number,selected,price,key) {
      if (selected<=number) {
        this.$dialog.confirm({
          title: 'Privacy Politics',
          message: name +  '</br>ราคา &nbsp;&nbsp;' + price + '</br>จำนวน &nbsp;&nbsp;' + selected + '</br>ราคารวม &nbsp;&nbsp;' + (selected*price),
          onConfirm: () => {
          firebase.database().ref('/stock/').child(key).update({
            number: number - selected
          })
          this.pullData()
        }
        })
        this.pullData()
      }
      else {
        this.$dialog.alert({
          title: 'Error',
          message: 'Insufficient Product',
          type: 'is-danger',
          hasIcon: true,
          icon: 'times-circle',
          iconPack: 'fa'
        })
        this.pullData()
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.order{
  margin-left: 20%;
  width: 60%;
}
</style>
