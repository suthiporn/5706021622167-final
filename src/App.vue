<template>
  <div id="app">

    <br><br><br>
<center>
    <table border="0" style="width:1300px;text-align:center;margin-top:20px;" >
      <tr>
        <td width="300px"><div style="width:290px;display:inline-block;background-color:#FFDAB9;"> # </div> <div style="width:290px;display:inline-block;;background-color:#FFDAB9;"> วิชา </div>
          <div style="width:290px;display:inline-block;;background-color:#FFDAB9;"> หน่วยกิต </div>
          <div style="width:290px;display:inline-block;;background-color:#FFDAB9;"> คะแนน </div>
          <div style="width:110px;display:inline-block;;background-color:#FFDAB9;color:red"> Change </div>
          <div style="width:110px;display:inline-block;;background-color:#FFDAB9;color:white"></div>
        </td>
      </tr>
      <tr>
        <td><hr>
        </td>
      </tr>
          <div v-for="(show,index) in information">
            <tr>
            <td width="300px">
            {{index+1}}
            </td>
            <td width="300px">
            <div  v-if="!show.update">{{show.name}}</div>
            <input type="text" style="width:250px;" v-model="subjectedit" v-if="show.update">
            </td>
            <td width="300px">
            <div  v-if="!show.update">{{show.tel}}</div>
            <input type="text" style="width:250px;" v-model="unitedit" v-if="show.update">
            </td>
            <td width="300px">
            <div  v-if="!show.update">{{show.email}}</div>
            <input type="text" style="width:250px;" v-model="gradeedit" v-if="show.update">
            </td>
            <td width="50px">
            <img src="https://cdn3.iconfinder.com/data/icons/softwaredemo/PNG/256x256/DeleteRed.png" @click="removeData(show.id)" style="width:30px;cursor:pointer">
            </td>
            <td width="50px">
            <img src="http://icons.iconarchive.com/icons/custom-icon-design/flatastic-1/128/edit-icon.png" @click="updateData(show.id)" v-if="show.updateButton" style="width:30px;cursor:pointer">
            <img src="http://download.seaicons.com/icons/paomedia/small-n-flat/1024/sign-check-icon.png" @click="savedata(show.id,subjectedit, unitedit, gradeedit)" v-if="show.save" style="width:30px;cursor:pointer">
            </td>
            </tr>
            <hr>
          </div>
    </table>
    <div style="margin-left:100px;margin-top:50px;">
      <input type="text" style="width:250px;height:30px;border-radius:10px;font-size:16px;" v-model="name" placeholder="ชื่อวิชา">
     <input type="number" max="3" min="1" style="width:250px;height:30px;border-radius:10px;font-size:16px;" v-model="tel" placeholder="หน่วยกิต">
     <input type="number" max="100" min="0" style="width:250px;height:30px;border-radius:10px;font-size:16px;" v-model="email" placeholder="คะแนน">
    <button @click="adddata()" style="width:100px;height:30px;cursor:pointer;border-radius:10px;font-size:16px;"> เพิ่ม </button></div>
    <br><br>
      GPA : {{gpa}}
    <br><br><button style="width:100px;height:30px;cursor:pointer;border-radius:10px;font-size:16px;" @click="calgrade()"> คำนวณเกรด </button>
    <button style="width:100px;height:30px;cursor:pointer;border-radius:10px;font-size:16px;" @click="reset()"> reset </button>
</center>
    <br><br><br>
    <br>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      information: [],
      name: '',
      gpa: 0,
      sumscoretel: 0,
      tel: '',
      email: '',
      scoretel: [],
      calunitgrade: 0,
      calunit: 0,
      calsum: 0.0,
      sumtel: 0,
      subjectedit: '',
      unitedit: '',
      gradeedit: '',
      fontweight: '',
      countfontweight: 0,
      colorBGfontweight: '',
      countdata: 0,
      fontcolor: '',
      fontstyle: '',
      fontstyleI: '',
      countfontstyle: '',
      colorBGfontstyle: '',
      fontunderline: '',
      countfontunderline: '',
      colorBGfontunderline: '',
      sumscore: 0
    }
  },
  methods: {
    adddata () {
      var data = {
        id: Date.now(),
        name: this.name,
        tel: this.tel,
        email: this.email,
        update: false,
        updateButton: true,
        save: false
      }
      if (this.name !== '' && this.tel !== '' && this.email !== '') {
        this.information.push(data)
        this.countdata += 1
      }
      this.name = ''
      this.tel = ''
      this.email = ''
    },
    removeData (idData) {
      var index = this.information.findIndex(information => information.id === idData)
      this.information.splice(index, 1)
      this.countdata -= 1
    },
    calgrade () {
      for (var i = 0; i < this.information.length; i++) {
        this.sumtel += this.information[i].tel
        this.scoretel[i] = (this.information[i].email * this.information[i].tel)
        if (this.information[i].email >= 0 && this.information[i].email < 50) {
          this.information[i].email = 'F'
        } else if (this.information[i].email >= 51 && this.information[i].email < 55) {
          this.information[i].email = 'D'
        } else if (this.information[i].email >= 56 && this.information[i].email < 60) {
          this.information[i].email = 'D+'
        } else if (this.information[i].email >= 61 && this.information[i].email < 65) {
          this.information[i].email = 'C'
        } else if (this.information[i].email >= 66 && this.information[i].email < 70) {
          this.information[i].email = 'C+'
        } else if (this.information[i].email >= 71 && this.information[i].email < 75) {
          this.information[i].email = 'B'
        } else if (this.information[i].email >= 76 && this.information[i].email < 80) {
          this.information[i].email = 'B+'
        } else if (this.information[i].email >= 80 && this.information[i].email <= 100) {
          this.information[i].email = 'A'
        }
      }
      for (var y = 0; y < this.scoretel.length; y++) {
        this.sumscoretel += this.scoretel[y]
      }

      this.gpa = this.sumscoretel / this.sumtel
    },
    reset () {
      this.name = ''
      this.gpa = 0
      this.sumscoretel = 0
      this.tel = ''
      this.email = ''
      this.scoretel = []
      this.calunitgrade = 0
      this.calunit = 0
      this.calsum = 0.0
      this.sumtel = 0
      this.subjectedit = ''
      this.unitedit = ''
      this.gradeedit = ''
      this.fontweight = ''
      this.countfontweight = 0
      this.colorBGfontweight = ''
      this.countdata = 0
      this.fontcolor = ''
      this.fontstyle = ''
      this.fontstyleI = ''
      this.countfontstyle = ''
      this.colorBGfontstyle = ''
      this.fontunderline = ''
      this.countfontunderline = ''
      this.colorBGfontunderline = ''
      this.sumscore = 0
    },
    cal () {
      this.calunitgrade = 0
      this.calsum = 0
      this.calunit = 0
      for (var i = 0; i < this.information.length; i++) {
        this.calunitgrade += this.information[i].sum
        this.calunit += this.information[i].tel
      }
      this.calsum = this.calunitgrade / this.tel
    },
    updateData (idData) {
      var index = this.information.findIndex(information => information.id === idData)
      this.information[index].updateButton = false
      this.information[index].update = true
      this.information[index].save = true
    },
    savedata (idData, subjectData, unitData, gradeData) {
      var index = this.information.findIndex(information => information.id === idData)
      this.information[index].updateButton = true
      this.information[index].update = false
      this.information[index].save = false
      this.information[index].name = subjectData
      this.information[index].tel = unitData
      this.information[index].email = gradeData
      this.information[index].sum = unitData * gradeData
      this.subjectedit = ''
      this.unitedit = ''
      this.gradeedit = ''
    },
    clear () {
      this.information.splice(0, this.information.length)
      this.name = ''
      this.tel = ''
      this.email = ''
      this.calunitgrade = 0
      this.calunit = 0
      this.calsum = 0.0
      this.subjectedit = ''
      this.unitedit = ''
      this.gradeedit = ''
    },
    fontweightFunc (count) {
      if (count % 2 === 0) {
        this.fontweight = ''
        this.colorBGfontweight = ''
      } else {
        this.fontweight = 'bold'
        this.colorBGfontweight = 'gray'
      }
    },
    fontstyleFunc (count) {
      if (count % 2 === 0) {
        this.fontstyleI = ''
        this.colorBGfontstyle = ''
      } else {
        this.fontstyleI = 'italic'
        this.colorBGfontstyle = 'gray'
      }
    },
    fontunderlineFunc (count) {
      if (count % 2 === 0) {
        this.fontunderline = ''
        this.colorBGfontunderline = ''
      } else {
        this.fontunderline = 'underline'
        this.colorBGfontunderline = 'gray'
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
