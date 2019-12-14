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
              <button class="panel_item" id="machine">
                <img src="./assets/image/machine.png" alt="machine" @click.self="setTool1"/>
              </button>
            </div>
            <div class="panel_box">
              <button class="panel_item" id="problemDomain">
                <img src="./assets/image/problemDomain.png" alt="pD" @click.self="setTool2"/>
              </button>
            </div>
            <div class="panel_box">
              <button class="panel_item" id="interface">
                <img src="./assets/image/interface.png" alt="interface" @click.self="setTool3"/>
              </button>
            </div>
            <div class="panel_box">
              <button class="panel_item" id="requirement">
                <img src="./assets/image/requirement.png" alt="requirement" @click.self="setTool4"/>
              </button>
            </div>
            <div class="panel_box">
              <button class="panel_item" id="inference">
                <img src="./assets/image/inference.png" alt="interface" @click.self="setTool5"/>
              </button>
            </div>
            <div class="panel_box">
              <button class="panel_item" id="constraint">
                <img src="./assets/image/constraint.png" alt="constrainst"  @click.self="setTool6"/>
              </button>
            </div>
          </div>
          <!--          plotboard-->
          <!--          <div class="plotboard" v-html="canvasEl"></div>-->
          <div id="plotboard"></div>
          <!--          rightbar-->
          <div class="rightbar">
            <div class="stepbar">
              <div style="text-align: center; font-size:22px; margin:3px">
                <b>Steps</b>
              </div>
              <div style="height: 225px">
                <el-steps direction="vertical" :active="step_active" space="24px">
                  Step1: ContextDiagram
                  <el-step title="1.1 Identify Machine"></el-step>
                  <el-step title="1.2 Identify Domains"></el-step>
                  <el-step title="1.3 Identify Interactions"></el-step>
                  <el-step title="1.4 Check Context "></el-step>
                  Step2: ProblemDiagram
                  <el-step title="2.1 Identify Requirement"></el-step>
                  <el-step title="2.2 Identify References"></el-step>
                  <el-step title="2.3 Check Problem"></el-step>
                </el-steps>
              </div>
              <el-button style="margin-top: auto; float:left" @click="stepBack">back</el-button>
              <el-button style="margin-top: auto; float:right" @click="stepNext">next</el-button>
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
                  <el-option v-for="item in pDEditor.ppOption" :key="item.value" :value="item.value"></el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="Domain Type" label-width="125px">
                <el-select v-model="pDEditor.dT" placeholder="请选择">
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

<script lang="ts">
import { Vue, Component } from "vue-property-decorator";
import $ from "jquery";
import Hammer from "hammerjs";

import { fabric } from "fabric";

// var tool_num:number = 0;

@Component({
  mounted(): void {
    // let hammertime = new Hammer(document.getElementById("plotboard"));
    // hammertime.on("tap", e => console.log(e));
  }
})
export default class App extends Vue {
  tool_num = 0;
  diagram: Diagram;
  canvas: any;
  step_active = -1;
  machineDisabled = false;
  machine: Machine;
  problemDomain = 1;
  requirementDisabled = false;
  problemDomains = new Array<ProblemDomain>();
  //button 是否可用
  btn = {
    machine: false,
    problemDomain: false,
    interfaceLine: false,
    requirement: false,
    inferenceLine: false,
    constraint: false
  }
  isContextCompleted = false;

  // click new button
  dlgNew = {
    dlgVisible: false,
    desc: ""
  };

  type = [
    {
      value: "Event"
    },
    {
      value: "State"
    },
    {
      value: "value"
    }
  ];

  mEditor = {
    dlgVisible: false,
    desc: "",
    sN: ""
  };

  pDEditor = {
    dlgVisible: false,
    desc: "",
    sN: "",
    pp: "",
    ppOption: [
      {
        value: "GivenDomain"
      },
      {
        value: "DesignDomian"
      }
    ],
    dT: "",
    dTOption: [
      {
        value: "Causal"
      },
      {
        value: "Biddable"
      },
      {
        value: "Lexical"
      }
    ]
  };

  rEditor = {
    dlgVisible: false,
    desc: ""
  };

  interfaceEditor = {
    name: "",
    dlgVisible: false,
    initiator: "",
    receiver: "",
    phe: "",
    type: "",
    pheList: []
  };

  referenceEditor = {
    name: "",
    dlgVisible: false,
    initiator: "",
    receiver: "",
    phe: "",
    type: "",
    constraint: false,
    pheList: []
  };

  constraintEditor = {
    name: "",
    dlgVisible: false,
    initiator: "",
    receiver: "",
    phe: "",
    type: "",
    constraint: false,
    pheList: []
  };

  setTool1(){
    this.tool_num = 1;
  }
  setTool2(){
    this.tool_num = 2;
  }
  setTool3(){
    this.tool_num = 3;
  }
  setTool4(){
   this.tool_num = 4;
  }
  setTool5(){
    this.tool_num = 5;
  }
  setTool6(){
    this.tool_num = 6;
  }


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
    this.diagram.createCanvas();
    this.dlgNew.desc = "";
    this.canvas = new fabric.Canvas("canvas");
    var func = function (options) {
      console.log(this.tool_num);
      switch(this.tool_num){
        case 0 : return;
        case 1 : this.addMachine(options.e.offsetX, options.e.offsetY); break;
        case 2 : this.addProblemDomain(options.e.offsetX, options.e.offsetY); break;

        case 4 : this.addRequirement(options.e.offsetX, options.e.offsetY); break;
      }
      this.tool_num = 0;
    }.bind(this);

    this.canvas.on('mouse:down', func);
  }

  addMachine( x, y) {
    if(this.btn.machine){
      if (!this.machineDisabled) {
        this.machine = new Machine(x, y);
        this.machine.draw(this.canvas);
        this.machineDisabled = true;
      }
    }
  }
  addProblemDomain(x, y) {
    if(this.btn.problemDomain){

      let pD = new ProblemDomain(x, y);
      this.problemDomains.push(pD);
      pD.draw(this.canvas);
    }
  }
  addRequirement(x ,y) {
    if(this.btn.requirement){
      if (!this.requirementDisabled) {
        let rqm = new Requirement(x ,y);
        rqm.draw(this.canvas);
        this.requirementDisabled = true;
      }
    }
  }


  //add Line
  addInterfaceLine(){
    if(this.btn.interfaceLine){
      alert(3)
    }
  }
  addInference(){
    if(this.btn.inferenceLine){
      alert(6)
    }
  }
  addConstraint(){
    if(this.btn.constraint){
      alert(6)
    }
  }


  // Machine Editor Editor
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
    this.machine.description = this.mEditor.desc;
    this.machine.shortName = this.mEditor.sN;
  }

  //Problem Domain Editor
  openPDEditor() {
    this.pDEditor.dlgVisible = true;
  }
  closePDEditor() {
    this.pDEditor.dlgVisible = false;
  }
  closePDEditorAndSave() {
    this.pDEditor.dlgVisible = false;
  }

  //Requirement Editor
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


  //interface Editor
  openInterfaceEditor() {
    this.interfaceEditor.dlgVisible = true;
  }
  closeInterfaceEditor() {
    this.interfaceEditor.dlgVisible = false;
  }

  // reference Editor

  // constraint Editor


  stepBack() {
    if (this.step_active > -1) {
      switch(this.step_active){
        case 6: this.btn.constraint = true; this.btn.inferenceLine = true; break;
        case 5: this.btn.constraint = false; this.btn.inferenceLine = false; this.btn.requirement = true; break;
        case 4: this.btn.requirement = false; break;
        case 3: this.btn.interfaceLine = true; break;
        case 2: this.btn.interfaceLine = false; this.btn.problemDomain = true; break;
        case 1: this.btn.problemDomain = false; this.btn.machine = true; break;
        case 0: this.btn.machine = false; break;
      }
      this.step_active--;
    }
  }
  stepNext() {
    if (this.step_active < 3) {
      this.step_active++;
      switch (this.step_active) {
        case 0: this.btn.machine = true; break;
        case 1: this.btn.machine = false; this.btn.problemDomain= true; break;
        case 2: this.btn.problemDomain = false; this.btn.interfaceLine = true; break;
        case 3: this.btn.interfaceLine = false; this.checkContext(); break;
      }
    }else if(this.isContextCompleted && this.step_active < 6){
      this.step_active++;
      switch (this.step_active) {
        case 4: this.btn.interfaceLine = false; this.btn.requirement = true; break;
        case 5: this.btn.requirement = false; this.btn.constraint = true; this.btn.inferenceLine = true; break;
        case 6: this.btn.constraint = false; this.btn.inferenceLine = false; this.checkProblem(); break;
      }

    }
  }

  checkContext(){
    this.isContextCompleted = true;
  }
  checkProblem(){

  }
}

class Diagram {
  name: string;
  constructor(name) {
    this.name = name;
  }
  createCanvas() {
    let width = $("#plotboard").width();
    let height = $("#plotboard").height();
    $("#plotboard").empty();
    $("#plotboard").css({ background: "#fff", opacity: "1.0" });
    let text = "<canvas id='canvas' " + "width='" + width + "' height='" + height + "'></canvas>";
    $("#plotboard").html(text);
  }
}

class CanvasComponent {

}

class Shape extends CanvasComponent {
  public description: string = "";

  constructor() {
    super();
  }
}

class Machine extends Shape {
  public description: string = "Machine";
  public shortName: string = "M";
  public  machine: fabric.Group;
  public left: number;
  public top: number;
  public width: number = 0;
  public height: number = 0;


  constructor(x, y) {
    super();
    this.left = x;
    this.top = y;
  }
  resetInfo(desc: string, shortName: string) {
    this.description = desc;
    this.shortName = shortName;
  }
  createGroup() {
    let rect = new fabric.Rect({
      fill: "#ccc",
      width: 150,
      height: 70,
      strokeWidth: 3,
      stroke: "black"
    });
    let rect2 = new fabric.Rect({
      fill: "#ccc",
      left: 10,
      width: 10,
      height: 70,
      strokeWidth: 3,
      stroke: "black"
    })
    let text = new fabric.Text(this.description + "\n(" + this.shortName +")",{
      fontSize: 16,
      top: 20,
      left: 30,
      fill: "black",
      strokeWidth: 2,
      textAlign: "center"
    })
    this.machine = new fabric.Group([rect,rect2,text],{
      top: this.top,
      left:this.left
    });
    let func = function () {
      this.left = this.machine.left;
      this.top = this.machine.top;
      // console.log(this.left, this.top);
    }.bind(this);
    this.machine.on("moving", func);

  }
  draw(canvas) {
    this.createGroup();
    canvas.add(this.machine);
    canvas.renderAll();
  }
}

class ProblemDomain extends Shape {
  public description: string = "ProblemDomain";
  public shortName: string = "PD";
  public physicalPropety: string;
  public domainType: string;
  public left;
  public top;
  public problemDomain: fabric.Group;

  constructor(x, y) {
    super();
    this.left = x;
    this.top = y;
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
  createGroup() {
    let rect = new fabric.Rect({
      fill: "#ccc",
      width: 150,
      height: 70,
      strokeWidth: 3,
      stroke: "black"
    });
    // let rect2 = new fabric.Rect({
    //   fill: "#ccc",
    //   left: 10,
    //   width: 10,
    //   height: 70,
    //   strokeWidth: 3,
    //   stroke: "black"
    // })
    let text = new fabric.Text(this.description + "\n(" + this.shortName +")",{
      fontSize: 16,
      top: 20,
      left: 20,
      fill: "black",
      strokeWidth: 2,
      textAlign: "center"
    })
    this.problemDomain = new fabric.Group([rect,text],{
      top: this.top,
      left:this.left
    });
    let func = function () {
      this.left = this.problemDomain.left;
      this.top = this.problemDomain.top;
      // console.log(this.left, this.top);
    }.bind(this);
    this.problemDomain.on("moving", func);
  }

  draw(canvas) {
    this.createGroup();
    canvas.add(this.problemDomain);
    canvas.renderAll();
  }
}

class Requirement extends Shape {
  public description = "Requirement";
  public left = 0;
  public top = 0;
  requirement :fabric.Group;
  constructor(x ,y) {
    super();
    this.left = x;
    this.top = y;
  }
  resetInfo(description: string) {
    this.description = description;
  }
  createGroup(){
    let ellipse = new fabric.Ellipse({

      fill: "#ccc",
      originX: "center",
      originY: "center",
      rx: 75,
      ry: 35,
      strokeDashArray: [5, 5],
      strokeWidth: 6,
      stroke: "black"
    });
    let text = new fabric.Text(this.description, {
      fontSize: 16,
      fill: "black",
      originX: "center",
      originY: "center",
      strokeWidth: 2,
    });
    this.requirement = new fabric.Group([ellipse, text],{
      top: this.top,
      left: this.left
    });
    let func = function () {
      this.left = this.requirement.left;
      this.top = this.requirement.top;
      // console.log(this.left, this.top);
    }.bind(this);
    this.requirement.on("moving", func);
  }

  // 18px的字宽为13
  draw(canvas) {
    this.createGroup();
    canvas.add(this.requirement);
    canvas.renderAll();
  }
}

function drawArrowBase (fromX, fromY, toX, toY, theta, headlen) {
  theta = typeof theta !== 'undefined' ? theta : 30;
  headlen = typeof theta !== 'undefined' ? headlen : 10;
  // 计算各角度和对应的P2,P3坐标
  let angle = (Math.atan2(fromY - toY, fromX - toX) * 180) / Math.PI,
          angle1 = ((angle + theta) * Math.PI) / 180,
          angle2 = ((angle - theta) * Math.PI) / 180,
          topX = headlen * Math.cos(angle1),
          topY = headlen * Math.sin(angle1),
          botX = headlen * Math.cos(angle2),
          botY = headlen * Math.sin(angle2);
  let arrowX = fromX - topX,
          arrowY = fromY - topY;
  let path = ' M ' + fromX + ' ' + fromY;
  path += ' L ' + toX + ' ' + toY;
  arrowX = toX + topX;
  arrowY = toY + topY;
  path += ' M ' + arrowX + ' ' + arrowY;
  path += ' L ' + toX + ' ' + toY;
  arrowX = toX + botX;
  arrowY = toY + botY;
  path += ' L ' + arrowX + ' ' + arrowY;
  return path;
}

class Line extends CanvasComponent {
  public src;
  public des;
  public init;
  public rec;
  public typeName = "Event";
  public phe;
  public pheList = [];
  public x1 = 0;
  public y1 = 0;
  public x2 = 0;
  public y2 = 0;
  public name = "";
}

class InterfaceLine extends Line {
  draw(canvas){
    let x1 = this.src.left + 75;
    let y1 = this.src.top + 35;
    let x2 = this.des.left + 75;
    let y2 = this.des.top + 35;
    this.x1 = x1;
    this.y1 = y1;
    this.x2 = x2;
    this.y2 = y2;
    let line = new fabric.Line([x1,y1,x2,y2],{
      fill:"black",
      stroke:"black",
      strokeWidth:2,
      //preserveObjectStacking:true,
      selectable:false,
    });
    let text = new fabric.Text(this.name,{
      fontSize: 16,
      left: (x1+x2)/2,
      top: (y1+y2)/2,
      originX: 'center',
      originY: 'center',
      //preserveObjectStacking:true,
    });
    let group = new fabric.Group([line, text], {
      //preserveObjectStacking:true,
      selectable: false
    });
    canvas.add(group);
    group.sendToBack();
    canvas.renderAll();
  }
}

class ReferenceLine extends Line {
  public cons = false;
  constructor() {
    super();
  }
  draw(canvas)
  {
    let x1 = this.src.left + 75;
    let y1 = this.src.top + 35;
    let x2 = this.des.left + 75;
    let y2 = this.des.top + 35;
    this.x1 = x1;
    this.y1 = y1;
    this.x2 = x2;
    this.y2 = y2;
    let line = new fabric.Line([x1,y1,x2,y2],{
      fill:"black",
      stroke:"black",
      strokeWidth:2,
      //preserveObjectStacking:true,
      selectable:false,
      strokeDashArray:[10,5],
    });
    let text = new fabric.Text(this.name,{
      fontSize: 16,
      left: (x1+x2)/2,
      top: (y1+y2)/2,
      originX: 'center',
      originY: 'center',
      //preserveObjectStacking:true,
    });
    let group = new fabric.Group([line, text], {
      //preserveObjectStacking:true,
      selectable: false
    });
    canvas.add(group);
    group.sendToBack();
    canvas.renderAll();
  }
}

class ConstraintLine extends Line {
  public cons = false;

  draw(canvas)
  {
    let x1 = this.src.left + 75;
    let y1 = this.src.top + 35;
    let x2 = this.des.left + 75;
    let y2 = this.des.top + 35;
    if(x1 > x2){x2 = x2 + 75;}
    else{x2 = x2 - 75;}
    if(y1 > y2){y2 = y2 + 35;}
    else{y2 = y2 - 35;}
    this.x1 = x1;
    this.y1 = y1;
    this.x2 = x2;
    this.y2 = y2;
    let theta = 20;
    let healen = 20;
    let line = new fabric.Path(drawArrowBase(x1,y1,x2,y2,theta,healen),{
      fill:"black",
      stroke:"black",
      strokeWidth:2,
      //preserveObjectStacking:true,
      selectable:false,
      strokeDashArray:[10,5],
      // x1:x1,
      // y1:y1,
      // x2:x2,
      // y2:y2,
    });
    let text = new fabric.Text(this.name,{
      fontSize: 16,
      left: (x1+x2)/2,
      top: (y1+y2)/2,
      originX: 'center',
      originY: 'center',
      //preserveObjectStacking:true,
    });
    let group = new fabric.Group([line, text], {
      //preserveObjectStacking:true,
      selectable: false
    });
    canvas.add(group);
    group.sendToBack();
    canvas.renderAll();
  }
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
#plotboard {
  background: #fff;
  float: left;
  width: 1000px;
  height: 100%;
  border: 3px solid #d1ba74;
  margin: 3px;
  opacity: 50%;
  overflow: scroll;
}

/*#plotboard .canvas {*/
/*  background: #fff;*/
/*  opacity: 1;*/
/*}*/

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

/*.rightbar .stepbar .step_content {*/
/*  height: 20px;*/
/*  mangin: 2px 0;*/
/*  color: #000000;*/

/*}*/

/*.rightbar .stepbar .step_controller {*/
/*  margin: 10px 0;*/
/*  !*float: right;*!*/
/*  font-size: 20px;*/
/*  text-align: center;*/
/*}*/

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
