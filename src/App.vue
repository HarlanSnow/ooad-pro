<template>
  <div id="app">
    <div>
      <div class="app-style">
        <div class="header">
          <a href="IPlot.html"
            ><img class="img" src="@/assets/image/iplot.png" alt="icon"
          /></a>
          <nav>
            <div>
              <ul>
                <li><button id="nav_bar_new"  @click.self="openDlgNew">New</button></li>
                <li>
                  <button id="nav_bar_open" >Open</button>
                </li>
                <li>
                  <button id="nav_bar_save" >Save</button>
                </li>
              </ul>
            </div>
          </nav>
        </div>

        <div class="body">
          <div class="toolbar">
            <div class="panel_box">
              <button class="panel_item" id="machine">
                <img src="@/assets/image/machine.png" alt="machine" />
              </button>
            </div>
            <div class="panel_box">
              <button class="panel_item" id="problemDomain">
                <img src="@/assets/image/problemDomain.png" alt="pD" />
              </button>
            </div>
            <div class="panel_box">
              <button class="panel_item" id="interface">
                <img src="@/assets/image/interface.png" alt="interface" />
              </button>
            </div>
            <div class="panel_box">
              <button class="panel_item" id="requirement">
                <img src="@/assets/image/requirement.png" alt="requirement" />
              </button>
            </div>
            <div class="panel_box">
              <button class="panel_item" id="inference">
                <img src="@/assets/image/inference.png" alt="interface" />
              </button>
            </div>
            <div class="panel_box">
              <button class="panel_item" id="constraint">
                <img src="@/assets/image/constraint.png" alt="constrainst" />
              </button>
            </div>
          </div>

          <div class="plotboard" v-html="canvasEl"></div>

          <el-dialog title="New a Diagram" :visible.sync="dlgNew.dlgNewVisible" >
            <el-form>
              <el-form-item label="Descriptin" label-width="80px">
                <el-input autocomplete="off" v-model="dlgNew.desc" clearable ></el-input>
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="closeDlgNew">cancel</el-button>
              <el-button type="primary" @click="closeDlgAndNewCanvas">comfirm</el-button>
            </div>
          </el-dialog>


          <div class="rightbar">
            <div class="stepbar">
              <div style="text-align: center; font-size:24px; margin: 5px;">
                <b>Steps</b>
              </div>
              <div class="step1">
                <div class="step_title">Step1: ContextDiagram</div>
                <ul style="list-style: none;">
                  <li><div class="step_content">1.1 Identify Machine</div></li>
                  <li><div class="step_content">1.2 Identify Domains</div></li>
                  <li>
                    <div class="step_content">1.3 Identify Interactions</div>
                  </li>
                  <li>
                    <div id="check_cD" class="step_content">
                      1.4 Check Context Diagram
                    </div>
                  </li>
                </ul>
              </div>

              <div class="step2">
                <div class="step_title">Step2: ProblemDiagram</div>
                <ul style="list-style: none;">
                  <li>
                    <div class="step_content">2.1 Identify Requirement</div>
                  </li>
                  <li>
                    <div class="step_content">2.2 Identify references</div>
                  </li>
                  <li>
                    <div id="check_pD" class="step_content">
                      2.3 Check Problem Diagram
                    </div>
                  </li>
                </ul>
              </div>

              <div class="step_controller">
                <div class="step_button"><button id="back">Back</button></div>
                <div class="step_button"><button id="next">Next</button></div>
              </div>
<!--              <div id="newdlg" v-show="flagNew"><b></b></div>-->
            </div>

            <div class="infobar">
              info
              <div class="info_content" id="phenomenon">
                Phenomenon
                <div id="phenomenon_content">hello</div>
              </div>
              <div class="info_content" id="interaction">
                Interaction
                <div id="interaction_content">hello</div>
              </div>
              <div class="info_content" id="reference">
                Reference
                <div id="reference_content">hello</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from "vue-property-decorator";
declare var jQuery: (selector: string) => any;


@Component({})
export default class App extends Vue {
  canvas: Canvas;
  canvasEl: string = "";

  dlgNew = {
    dlgNewVisible: false,
    desc : ""
  }

  openDlgNew(){
    this.dlgNew.dlgNewVisible = true;
  }
  closeDlgNew(){
    this.dlgNew.dlgNewVisible = false;
    this.dlgNew.desc = "";
  }
  closeDlgAndNewCanvas(){
    this.dlgNew.dlgNewVisible = false;
    this.canvas = new Canvas(this.dlgNew.desc);
    // console.log(this.canvas);
    this.canvasEl = this.canvas.createCanvas();
  }
}


class Canvas {
  name: string;
  constructor(name){
    this.name = name;
  }
  createCanvas(){
    let width = jQuery(".plotboard").width();
    let height = jQuery(".plotboard").height();
    jQuery(".plotboard").empty();
    jQuery(".plotboard").css({"background":"#fff","opacity":"1.0"});
    return "<canvas id='canvas' " +"width='" + width +"' height='" +height+"'></canvas>";
  }
}

class component{

}

class Line{

}

class InterfaceLine extends Line{

}

class ReferenceLine extends Line{

}

class ConstraintLine extends Line{

}

class Shape{

}

class Machine extends Shape{

}

class ProblemDomain extends Shape{

}

class Requirement extends Shape{

}

</script>

<style lang="scss">
* {
  font-family: arial;
  border: 0;
  padding: 0;
  margin: 0;
  color: #000;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}

html,
body {
  width: auto;
  height: 100%;
  font-size: 100%;
}

body {
  background: url("./assets/image/BG.jpg") no-repeat fixed center;
}

.app-style {
  position: absolute;
  height: 100%;
  width: 1320px;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: 0 auto;
  /*text-align:center;*/
}

/*上方菜单板*/
.header {
  /*background: #e6ceac;*/
  background: #d6d5b7;
  height: 12%;
  width: auto;
  font-size: 1.5em;
  text-align: center;
  margin: 0.3% 0 0 0;
  border: 3px solid #d1ba74;
  opacity: 85%;
}

.header .img {
  position: relative;
  top: 15%;
  float: left;
  height: 70%;
  left: 0.5%;
  margin: auto;
}

nav {
  float: left;
  position: relative;
  top: 35%;
  height: 30%;
}

nav ul {
  position: relative;
  list-style: none;
  text-align: center;
}

nav ul li {
  position: relative;
  float: left;
  color: black;
  font-size: 18px;
  width: 80px;
  margin: 0 10px;
}

.body {
  position: relative;
  height: 85%;
  bottom: 0;
  margin: 0 auto;
  display: flex;
}

/*左边工具板*/
.toolbar {
  background: #d6d5b7;
  /*top: 12%;*/
  float: left;
  width: 60px;
  height: 100%;
  border: 3px solid #d1ba74;
  margin: 3px;
  opacity: 80%;
  overflow: scroll;
}

.panel_box {
  position: relative;
  margin: 10px 2px;
  background: #000;
  width: 50px;
  height: 50px;
}

.panel_item {
  margin: auto;
  width: 50px;
  height: 50px;
  overflow: hidden;
  opacity: 1;
}

.panel_item img {
  width: 50px;
  height: 50px;
  opacity: 0.2;
  /*border: 1px solid black;*/
}

.panel_item img:hover {
  opacity: 1;
  border: 3px solid #f4606c;
}

/*中间绘图板*/
.plotboard {
  background: #fff;
  float: left;
  width: 1000px;
  height: 100%;
  border: 3px solid #d1ba74;
  margin: 3px;
  opacity: 50%;
  overflow: scroll;
}

.plotboard .canvas {
  background: #fff;
  opacity: 1;
}

.rightbar {
  position: relative;
  float: left;
  background: #d6d5b7;
  height: 100%;
  width: 240px;
  border: 3px solid #d1ba74;
  margin: 3px;
  opacity: 80%;
}

.rightbar .stepbar {
  position: relative;
  top: 0;
  background: #d6d5b7;
  height: 40%;
  border: 1px solid #d1ba74;
  margin: 1px;
  overflow: scroll;
}

.rightbar .stepbar .step_title {
  text-align: center;
  font-size: 20px;
  margin: 3px;
}

.rightbar .stepbar .step_content {
  height: 20px;
  mangin: 2px 0;
  color: #808080;
}

.rightbar .stepbar .step_controller {
  margin: 10px 0;
  /*float: right;*/
  font-size: 20px;
  text-align: center;
}

.rightbar .stepbar .step_controller .step_button {
  position: relative;
  float: left;
  width: 115px;
  /*border: 1px solid white;*/
  margin: 2px 0;
  /*padding: 1px;*/
}
.rightbar .stepbar .step_controller botton {
  background: #ecad9e;
  color: #777;
  /*background: #d1ba74;*/
}

.rightbar .stepbar .step_controller botton:hover {
  /*background: #000;*/
  color: #000;
  border: 2px solid #f4606c;
}
.rightbar .infobar {
  position: relative;
  background: #d6d5b7;
  height: 59%;
  border: 1px solid #d1ba74;
  margin: 1px;
  overflow: scroll;
}

.rightbar .infobar .info_content {
  background: white;
  margin: 2px 0;
  opacity: 0.8;
}

#dlg_new{
  position: absolute;
  background: #000;
  width:300px;
  height: 200px;
  margin: auto;
}
</style>