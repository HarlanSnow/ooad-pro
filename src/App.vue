<template>
  <div id="app">
    <div>
      <div class="app-style">
        <div class="header">
          <a href="index.html"
            ><img class="img" src="./assets/image/iplot.png" alt="icon"
          /></a>
          <nav>
            <div>
              <ul>
                <li>
                  <button id="nav_bar_new" @click.self="openDlgNew">New</button>
                </li>
                <li>
                  <button id="nav_bar_open">Open</button>
                </li>
                <li>
                  <button id="nav_bar_save">Save</button>
                </li>
              </ul>
            </div>
          </nav>
        </div>

        <div class="body">
          <!--          toolbar-->
          <div class="toolbar">
            <div class="panel_box">
              <button class="panel_item" id="machine" >
                <img src="./assets/image/machine.png" alt="machine" @click.self="addMachine" />
              </button>
            </div>
            <div class="panel_box">
              <button class="panel_item" id="problemDomain">
                <img src="./assets/image/problemDomain.png" alt="pD" />
              </button>
            </div>
            <div class="panel_box">
              <button class="panel_item" id="interface">
                <img src="./assets/image/interface.png" alt="interface" />
              </button>
            </div>
            <div class="panel_box">
              <button class="panel_item" id="requirement">
                <img src="./assets/image/requirement.png" alt="requirement" />
              </button>
            </div>
            <div class="panel_box">
              <button class="panel_item" id="inference">
                <img src="./assets/image/inference.png" alt="interface" />
              </button>
            </div>
            <div class="panel_box">
              <button class="panel_item" id="constraint">
                <img src="./assets/image/constraint.png" alt="constrainst" />
              </button>
            </div>
          </div>
          <!--          plotboard-->
          <div class="plotboard" v-html="canvasEl"></div>
          <!--          rightbar-->
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

          <!--          click New-->
          <el-dialog title="New a Diagram" :visible.sync="dlgNew.dlgVisible" width="25%">
            <el-form label-position="left">
              <el-form-item label="Description" label-width="100px">
                <el-input autocomplete="off" v-model="dlgNew.desc" clearable></el-input>
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="closeDlgNew">cancel</el-button>
              <el-button type="primary" @click="closeDlgAndNewCanvas">confirm</el-button>
            </div>
          </el-dialog>
          <!--          click machine-->
          <el-dialog title="Machine Editor" :visible.sync="mEditor.dlgVisible" width="25%">
            <el-form label-position="left">
              <el-form-item label="Description" label-width="100px">
                <el-input autocomplete="off" v-model="mEditor.desc" clearable></el-input>
              </el-form-item>
              <el-form-item label="ShortName" label-width="100px">
                <el-input autocomplete="off" v-model="mEditor.sN" clearable></el-input>
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="closeMEditor">cancel</el-button>
              <el-button type="primary" @click="closeMEditorAndSave">confirm</el-button>
            </div>
          </el-dialog>
          <!--          click requirement-->
          <el-dialog title="Requirement Editor" :visible.sync="rEditor.dlgVisible" width="25%">
            <el-form label-position="left">
              <el-form-item label="Descriptin" label-width="100px">
                <el-input autocomplete="off" v-model="rEditor.desc" clearable></el-input>
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="closeREditor">cancel</el-button>
              <el-button type="primary" @click="closeREditorAndSave">confirm</el-button>
            </div>
          </el-dialog>
          <!--          click problem domain-->
          <el-dialog title="Problem Domain Editor" :visible.sync="pDEditor.dlgVisible" width="25%">
            <el-form label-position="left">
              <el-form-item label="Description" label-width="125px">
                <el-input autocomplete="off" v-model="pDEditor.desc" clearable></el-input>
              </el-form-item>
              <el-form-item label="ShortName" label-width="125px">
                <el-input autocomplete="off" v-model="pDEditor.sN" clearable></el-input>
              </el-form-item>
              <el-form-item label="Physical Property" label-width="125px">
                <el-select v-model="pDEditor.pp" placeholder="请选择">
                  <el-option v-for="item in pDEditor.ppOption" :key="item.value" :value="item.value">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="Domain Type" label-width="125px">
                <el-select v-model="pDEditor.dT" placeholder="请选择" >
                  <el-option v-for="item in pDEditor.dTOption" :key="item.value" :value="item.value">
                  </el-option>
                </el-select>
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="closePDEditor">cancel</el-button>
              <el-button type="primary" @click="closePDEditorAndSave">confirm</el-button>
            </div>
          </el-dialog>
          <!--          click Interface Line-->
          <el-dialog title="Interface Editor" :visible.sync="interfaceEditor.dlgVisible" width="25%">
            <el-form label-position="left">
              <el-form-item label="Initiator" label-width="125px">
<!--                <el-select v-model="value" placeholder="请选择">-->
<!--                  <el-option v-for="item in interfaceEditor.initiator" :key="item.value" :value="item.value">-->
<!--                  </el-option>-->
<!--                </el-select>-->
              </el-form-item>
              <el-form-item label="Phenomenon" label-width="125px">
<!--                <el-select v-model="value" placeholder="请选择">-->
<!--                  <el-option v-for="item in interfaceEditor.pheList" :key="item.value" :value="item.value">-->
<!--                  </el-option>-->
<!--                </el-select>-->
              </el-form-item>
              <el-form-item label="Type" label-width="125px">
                <el-select v-model="interfaceEditor.type" placeholder="请选择">
                  <el-option v-for="item in interfaceEditor.types" :key="item.value" :value="item.value">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="Phenomenon List" label-width="125px">
<!--                <el-table :data="interfaceEditor.pheList" style="width: 100%">-->
<!--                  <el-table-column prop="init"></el-table-column>-->
<!--                  <el-table-column prop="phe"></el-table-column>-->
<!--                  <el-table-column prop="type"></el-table-column>-->
<!--                </el-table>-->
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="closePDEditor">cancel</el-button>
              <el-button type="primary" @click="closePDEditorAndSave">confirm</el-button>
            </div>
          </el-dialog>
        </div>
      </div>
    </div>
  </div>
</template>

<!--<script></script>-->

<script lang="ts">
import { Vue, Component } from "vue-property-decorator";
declare var jQuery: (selector: string) => any;

import { fabric } from "fabric";

@Component({})
export default class App extends Vue {
  diagram: Diagram;
  canvas:any;
  canvasEl: string = "";
  // click new button
  dlgNew = {
    dlgVisible: false,
    desc: ""
  };
  openDlgNew() {
    //click New
    this.dlgNew.dlgVisible = true;
  }
  closeDlgNew() {
    //cancel
    this.dlgNew.dlgVisible = false;
    this.dlgNew.desc = "";
  }
  closeDlgAndNewCanvas() {
    //comfirm
    this.dlgNew.dlgVisible = false;
    this.diagram = new Diagram(this.dlgNew.desc);
    this.canvasEl = this.diagram.createCanvas();
    this.canvas = new fabric.Canvas("canvas");
    console.log(this.canvas);
    this.dlgNew.desc = "";
  }

  addMachine(){
    let rect = new fabric.Rect({
      top : 50,
      left : 100,
      width : 100,
      height : 70,
      fill : "#000"
    });
    this.canvas.add(rect);
  }

  // Machine Editor Editor
  mEditor = {
    dlgVisible: false,
    desc: "",
    sN: ""
  };
  openMEditor() {
    //dblclick open
    this.mEditor.dlgVisible = true;
  }
  closeMEditor() {
    //cancel
    this.mEditor.dlgVisible = false;
    // this.mEditor.desc = "";
    // this.
  }
  closeMEditorAndSave() {
    //comfirm
    this.mEditor.dlgVisible = false;
  }
  //Problem Domain Editor
  pDEditor = {
    dlgVisible: false,
    desc: "",
    sN: "",
    pp: "",
    ppOption: [{
      value: "GivenDomain"
    },{
      value: "DesignDomian"
    }],
    dT: "",
    dTOption:[{
      value: "Causal"
    },{
      value: "Biddable"
    },{
      value: "Lexical"
    }]
  };
  openPDEditor(){
    this.pDEditor.dlgVisible = true;
  }
  closePDEditor(){
    this.pDEditor.dlgVisible = false;
  }
  closePDEditorAndSave(){
    this.pDEditor.dlgVisible = false;
  }
  //Requirement Editor
  rEditor = {
    dlgVisible: false,
    desc: ""
  };
  openREditor() {
    // dblclick requirement
    this.rEditor.dlgVisible = true;
  }
  closeREditor() {
    //cancel
    this.rEditor.dlgVisible = false;
    this.rEditor.desc = "";
  }
  closeREditorAndSave() {
    //comfirm
    this.rEditor.dlgVisible = false;
  }

  type = [{
    value: "Event"
  },{
    value: "State"
  },{
    value: "value"
  }];
  //interface Editor
  interfaceEditor = {
    dlgVisible: false,
    initiator: "",
    phe: "",
    types: this.type,
    type: "",
    pheList:[]
  };
  openInterfaceEditor(){
    this.interfaceEditor.dlgVisible = true;
  }
  closeInterfaceEditor(){
    this.interfaceEditor.dlgVisible = false;
  }
  // reference Editor
  referenceEditor = {
    dlgVisible: false,
    initiator: "",
    receiver: "",
    phe: "",
    types: this.type,
    type: "",
    constraint: false,
    pheList: []
  }
  // constraint Editor
  constraintEditor = {
    dlgVisible: false,
    initiator: "",
    receiver: "",
    phe: "",
    types: this.type,
    type : "",
    constraint: false,
    pheList: []
  }
}

class Diagram {
  name: string;
  constructor(name) {
    this.name = name;
  }
  createCanvas() {
    let width = jQuery(".plotboard").width();
    let height = jQuery(".plotboard").height();
    jQuery(".plotboard").empty();
    jQuery(".plotboard").css({ background: "#fff", opacity: "1.0" });
    return ("<canvas id='canvas' " + "width='" +  width + "' height='" + height + "'></canvas>");
  }
}

class component {}

class Line {}

class InterfaceLine extends Line {}

class ReferenceLine extends Line {}

class ConstraintLine extends Line {}

class Shape {
  public description: string = "";
}

class Machine extends Shape {
  public description: string = "Machine";
  public shortName: string = "M";

  constructor(description: string, shortName: string) {
    super();
    this.description = description;
    this.shortName = shortName;
  }
  resetInfo(desc: string, shortName: string) {
    this.description = desc;
    this.shortName = shortName;
  }
}

class ProblemDomain extends Shape {
  public description: string = "ProblemDomain";
  public shortName: string = "PD";
  public physicalPropety: string;
  public domainType: string;

  constructor() {
    super();
  }

  resetInfo(
    desc: string,
    shortName: string,
    physicalPropety: string,
    domainType: string
  ) {
    this.description = desc;
    this.shortName = shortName;
    this.physicalPropety = physicalPropety;
    this.domainType = domainType;
  }
}

class Requirement extends Shape {
  constructor(description: string) {
    super();
    this.description = description;
  }
  resetInfo(description: string) {
    this.description = description;
  }
}

// interface Phenomenon{
//   initiator: string;
//   interaction :string;
//   type: string;
// }
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
nav ul li button {
  background: #d6d5b7;
  font-size: 18px;
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
.rightbar .stepbar .step_controller button {
  background: #ecad9e;
  color: #777;
  font-size: 18px;
  /*background: #d1ba74;*/
}

.rightbar .stepbar .step_controller button:hover {
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
</style>
