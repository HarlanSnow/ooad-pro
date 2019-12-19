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
                  <button id="nav_bar_open" >Open</button>
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
              <button class="panel_item" id="reference">
                <img src="./assets/image/reference.png" alt="interface" @click.self="setTool5"/>
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
                  <b>Step1: ContextDiagram</b>
                  <el-step title="1.1 Identify Machine"></el-step>
                  <el-step title="1.2 Identify Domains"></el-step>
                  <el-step title="1.3 Identify Interactions"></el-step>
                  <el-step title="1.4 Check Context "></el-step>
                  <b>Step2: ProblemDiagram</b>
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
              <div class="info_content" id="phenomenoninfo">
                Phenomenon
                <div id="phenomenon_content"></div>
              </div>
              <div class="info_content" id="interactioninfo">
                Interaction
                <div id="interaction_content"></div>
              </div>
              <div class="info_content" id="referenceinfo">
                Reference
                <div id="reference_content"></div>
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
              <el-form-item label="Description" label-width="100px">
                <el-input autocomplete="off" v-model="rEditor.desc" clearable></el-input>
              </el-form-item>
              <el-form-item label="ShortName" label-width="100px">
                <el-input autocomplete="off" v-model="rEditor.sN" clearable></el-input>
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
          <el-dialog title="Interface Editor" :visible.sync="interfaceEditor.dlgVisible" width="30%">
            <el-form label-position="left">

              <el-form-item label="Initiator" label-width="125px">
                <el-select v-model="interfaceEditor.initiator" placeholder="请选择">
                  <el-option
                          v-for="item in options1"
                          :key="item.value"
                          :label="item.label"
                          :value="item.value">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="Receiver" label-width="125px">
                <el-select v-model="interfaceEditor.receiver" placeholder="请选择">
                  <el-option
                          v-for="item in options1"
                          :key="item.value"
                          :label="item.label"
                          :value="item.value">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="Phenomenon" label-width="125px">
                <el-input v-model="interfaceEditor.phe"></el-input>
              </el-form-item>
              <el-form-item label="Type" label-width="125px">
                <el-select v-model="interfaceEditor.type" placeholder="请选择">
                  <el-option v-for="item in Types" :key="item.value" :value="item.value">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="PhenomenonList" label-width="125px">
                <el-card class="box-card">
                  <div v-for="o in interfaceEditor.pheList" :key="o">
                    {{ o }}
                  </div>
                </el-card>
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="closeInterfaceEditor">cancel</el-button>
              <el-button type="warning" @click="add1">Add</el-button>
              <el-button type="primary" @click="closeInterfaceAndSave">confirm</el-button>
            </div>
          </el-dialog>

          <!--          click Reference Line-->
          <el-dialog title="Reference Editor" :visible.sync="referenceEditor.dlgVisible" width="30%">
            <el-form label-position="left">

              <el-form-item label="Initiator" label-width="125px">
                <el-select v-model="referenceEditor.initiator" placeholder="请选择">
                  <el-option
                          v-for="item in options2"
                          :key="item.value"
                          :label="item.label"
                          :value="item.value">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="Receiver" label-width="125px">
                <el-select v-model="referenceEditor.receiver" placeholder="请选择">
                  <el-option
                          v-for="item in options2"
                          :key="item.value"
                          :label="item.label"
                          :value="item.value">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="Phenomenon" label-width="125px">
                <el-input v-model="referenceEditor.phe"></el-input>
              </el-form-item>
              <el-form-item label="Type" label-width="125px">
                <el-select v-model="referenceEditor.type" placeholder="请选择">
                  <el-option v-for="item in Types" :key="item.value" :value="item.value">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="PhenomenonList">
                <el-card class="box-card">
                  <div v-for="o in referenceEditor.pheList" :key="o">
                    {{ o }}
                  </div>
                </el-card>
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="closeReferenceEditor">cancel</el-button>
              <el-button type="warning" @click="add2">Add</el-button>
              <el-button type="primary" @click="closeReferenceAndSave">confirm</el-button>
            </div>
          </el-dialog>


          <el-dialog title="Constraint Editor" :visible.sync="constraintEditor.dlgVisible" width="30%">
            <el-form label-position="left">

              <el-form-item label="Initiator" label-width="125px">
                <el-select v-model="constraintEditor.initiator" placeholder="请选择">
                  <el-option
                          v-for="item in options3"
                          :key="item.value"
                          :label="item.label"
                          :value="item.value">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="Receiver" label-width="125px">
                <el-select v-model="referenceEditor.receiver" placeholder="请选择">
                  <el-option
                          v-for="item in options3"
                          :key="item.value"
                          :label="item.label"
                          :value="item.value">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="Phenomenon" label-width="125px">
                <el-input v-model="constraintEditor.phe"></el-input>
              </el-form-item>
              <el-form-item label="Type" label-width="125px">
                <el-select v-model="constraintEditor.type" placeholder="请选择">
                  <el-option v-for="item in Types" :key="item.value" :value="item.value">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="PhenomenonList" label-width="125px">
                <el-card class="box-card" >
                  <div v-for="o in constraintEditor.pheList" :key="o" style="overflow: scroll">
                    {{ o }}
                  </div>
                </el-card>
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="closeConstraintEditor">cancel</el-button>
              <el-button type="warning" @click="add3">Add</el-button>
              <el-button type="primary" @click="closeConstraintAndSave">confirm</el-button>
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
import { fabric } from "fabric";



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
  canvasObj: Canvas;
  step_active = -1;
  machineDisabled = false;
  machine: Machine;
  problemDomain = 1;
  requirementDisabled = false;
  problemDomains = new Array<ProblemDomain>();
  src: Shape;
  des: Shape;
  content;
  index:number;
  //button 是否可用
  btn = {
    machine: false,
    problemDomain: false,
    interfaceLine: false,
    requirement: false,
    referenceLine: false,
    constraint: false
  }
  isContextCompleted = false;

  // click new button
  dlgNew = {
    dlgVisible: false,
    desc: ""
  };

  Types = [
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
        value: "DesignDomain"
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
    desc: "",
    sN: ""
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

  options1 = [];

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

  options2 = [];

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

  options3 = [];

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
    let func = function (options) {
      console.log(this.tool_num);
      let x = options.e.offsetX;
      let y = options.e.offsetY;
      // console.log(x, y);
      // console.log("click");
      if(this.tool_num === 0){
        return;
      }else if(this.tool_num === 1){
        this.addMachine(x, y);
        this.tool_num = 0;
      }else if(this.tool_num === 2){
        this.addProblemDomain(x, y)
        this.tool_num = 0;
      }else if(this.tool_num === 3){
        if(this.btn.interfaceLine) {
          let flag = 0;
          let data = this.canvasObj.componentList;
          for (let i = 0; i < data.length; i++) {
            if (data[i] instanceof Machine) {
              if (x >= data[i].left && x <= (data[i].left + data[i].width) && y >= data[i].top && y <= (data[i].height + data[i].top)) {
                this.src = data[i];
                flag = 1;
                break;
              }
            } else if (data[i] instanceof ProblemDomain) {
              if (x >= data[i].left && x <= (data[i].left + data[i].width) && y >= data[i].top && y <= (data[i].height + data[i].top)) {
                this.src = data[i];
                flag = 1;
                break;
              }
            }
          }
          if (flag == 0) {
            this.tool_num = 0;
            return;
          }
          this.tool_num = 31;
          // console.log(1111111);
        }
      }else if(this.tool_num === 31){
        let flag = 0;
        let data = this.canvasObj.componentList;
        for (let i = 0; i < data.length; i++) {
          if (data[i] instanceof Machine) {
            if (x >= data[i].left && x <= (data[i].left + data[i].width) && y >= data[i].top && y <= (data[i].height + data[i].top) && this.src != data[i]) {
              this.des = data[i];
              flag = 1;
              break;
            }
          } else if (data[i] instanceof ProblemDomain) {
            if (x >= data[i].left && x <= (data[i].left + data[i].width) && y >= data[i].top && y <= (data[i].height + data[i].top) && this.src != data[i]) {
              this.des= data[i];
              flag = 1;
              break;
            }
          }
        }
        if (flag == 0) {
          this.tool_num = 0;
          this.src = undefined;
          return;
        }
        let interfaceLine = new InterfaceLine();
        interfaceLine.src = this.src;
        interfaceLine.des = this.des;
        interfaceLine.name = "a";
        interfaceLine.init = interfaceLine.src.shortName;
        interfaceLine.rec = interfaceLine.des.shortName;
        // console.log(interfaceLine.init, interfaceLine.rec);
        interfaceLine.draw(this.canvas);
        this.canvasObj.addComponent(interfaceLine);
        this.tool_num = 0;
      }else if(this.tool_num === 4 ){
        this.addRequirement(x, y);
        this.tool_num = 0;
      }else if(this.tool_num === 5){
        if(this.btn.referenceLine) {
          let flag = 0;
          let data = this.canvasObj.componentList;
          for (let i = 0; i < data.length; i++) {
            // if (data[i] instanceof Requirement) {
            //   if (x >= data[i].left && x <= (data[i].left + data[i].width) && y >= data[i].top && y <= (data[i].height + data[i].top)) {
            //     this.src = data[i];
            //     flag = 1;
            //     break;
            //   }
            // } else
            if (data[i] instanceof ProblemDomain) {
              if (x >= data[i].left && x <= (data[i].left + data[i].width) && y >= data[i].top && y <= (data[i].height + data[i].top)) {
                this.src = data[i];
                flag = 1;
                break;
              }
            }
          }
          if (flag == 0) {
            this.tool_num = 0;
            return;
          }
          this.tool_num = 51;
        }
      }else if(this.tool_num === 51){
        let flag = 0;
        let data = this.canvasObj.componentList;
        for (let i = 0; i < data.length; i++) {
          if (data[i] instanceof Requirement) {
            if (x >= data[i].left && x <= (data[i].left + data[i].width) && y >= data[i].top && y <= (data[i].height + data[i].top) && this.src != data[i]) {
              this.des = data[i];
              flag = 1;
              break;
            }
          }
          // else if (data[i] instanceof ProblemDomain) {
          //   if (x >= data[i].left && x <= (data[i].left + data[i].width) && y >= data[i].top && y <= (data[i].height + data[i].top) && this.src != data[i]) {
          //     this.des = data[i];
          //     flag = 1;
          //     break;
          //   }
          // }
        }
        if (flag == 0) {
          this.tool_num = 0;
          this.src = undefined;
          return;
        }

        let referenceLine = new ReferenceLine();
        referenceLine.src = this.src;
        referenceLine.des = this.des;
        referenceLine.init = referenceLine.src.shortName;
        referenceLine.rec = referenceLine.des.shortName;
        referenceLine.name = "b";
        referenceLine.cons = false;
        referenceLine.draw(this.canvas);
        this.canvasObj.addComponent(referenceLine);
        this.tool_num = 0;
      } else if (this.tool_num === 6){
        if(this.btn.constraint) {
          let flag = 0;
          let data = this.canvasObj.componentList;
          for (let i = 0; i < data.length; i++) {
            if (data[i] instanceof Requirement) {
              if (x >= data[i].left && x <= (data[i].left + data[i].width) && y >= data[i].top && y <= (data[i].height + data[i].top)) {
                this.src = data[i];
                flag = 1;
                break;
              }
            }
          }
          if (flag == 0) {
            this.tool_num = 0;
            return;
          }
          this.tool_num = 61;
        }
      }else if(this.tool_num === 61) {
        let flag = 0;
        let data = this.canvasObj.componentList;
        for (let i = 0; i < data.length; i++) {

          if (data[i] instanceof ProblemDomain) {
            if (x >= data[i].left && x <= (data[i].left + data[i].width) && y >= data[i].top && y <= (data[i].height + data[i].top)) {
              this.des = data[i];
              flag = 1;
              break;
            }
          }
        }
        if (flag == 0) {
          this.tool_num = 0;
          this.src = undefined;
          return;
        }
        let constraintLine = new ConstraintLine();
        constraintLine.src = this.src;
        constraintLine.des = this.des;
        constraintLine.init = constraintLine.src.shortName;
        constraintLine.rec = constraintLine.des.shortName;
        constraintLine.name = "c";
        constraintLine.cons = false;
        constraintLine.draw(this.canvas);
        this.canvasObj.addComponent(constraintLine);
        this.tool_num = 0;
      }
    }.bind(this);
    this.canvas.on('mouse:down', func);
    this.canvasObj = new Canvas();

    let func2 = function (options) {
      let x = options.e.offsetX;
      let y = options.e.offsetY;
      console.log(x, y);
      // console.log("dblclick");
      let data = this.canvasObj.componentList;
      let flag = 0;
      for(let i = 0; i < data.length; i++){
        if (data[i] instanceof Requirement) {
          if (x >= data[i].left && x <= (data[i].left + data[i].width) && y >= data[i].top && y <= (data[i].height + data[i].top)) {
            //_this.index = i
            this.content = data[i]
            this.rEditor.desc = data[i].description;
            this.rEditor.sN = data[i].shortName;
            this.rEditor.dlgVisible = true;
            flag = 1;
            break;
          }
        } else if (data[i] instanceof ProblemDomain) {
          if (x >= data[i].left && x <= (data[i].left + data[i].width) && y >= data[i].top && y <= (data[i].height + data[i].top)) {
            //_this.index = i
            this.content = data[i]
            this.pDEditor.desc = data[i].description;
            this.pDEditor.sN = data[i].shortName;
            this.pDEditor.dlgVisible = true;
            flag = 1;
            break;
          }
        } else if (data[i] instanceof Machine) {
          if (x >= data[i].left && x <= (data[i].left + data[i].width) && y >= data[i].top && y <= (data[i].height + data[i].top)) {
            //_this.index = i
            this.content = data[i]
            this.mEditor.desc = data[i].description;
            this.mEditor.sN = data[i].shortName;
            this.mEditor.dlgVisible = true;
            flag = 1;
            break;
          }
        } else if (data[i] instanceof InterfaceLine) {
          let a = data[i].x1;
          let b = data[i].y1;
          let c = data[i].x2;
          let d = data[i].y2;
          let k = (d - b) / (c - a);
          let m = b - k * a;
          let Y = k * x + m;
          console.log(y, Y);
          if (a > c){
            let p = a;
            a = c;
            c = p;
          }
          this.interfaceEditor.name = data[i].name;
          this.options1 = [];
          this.options1.push({
            value: data[i].src.shortName,
            label: data[i].src.shortName
          })
          this.options1.push({
            value: data[i].des.shortName,
            label: data[i].des.shortName
          })
          this.interfaceEditor.initiator = data[i].init;
          this.interfaceEditor.receiver = data[i].rec;
          this.interfaceEditor.type = data[i].type;
          if (data[i].phe !== '') {
            this.interfaceEditor.phe = data[i].phe;
          } else {
            this.interfaceEditor.phe = '';
          }
          if (data[i].pheList.length !== 0) {
            this.interfaceEditor.pheList = data[i].pheList;
          } else {
            this.interfaceEditor.pheList = [];
          }
          if (y - Y <= 10 && Y - y <= 10 && x >= a && x <= c) {
            this.content = data[i];
            this.interfaceEditor.dlgVisible = true;
            flag = 2;
            break;
          }
        } else if (data[i] instanceof ReferenceLine) {
          let a = data[i].x1;
          let b = data[i].y1;
          let c = data[i].x2;
          let d = data[i].y2;
          let k = (d - b) / (c - a);
          let m = b - k * a;
          let Y = k * x + m;
          console.log(y, Y);
          if (a > c){
            let p = a;
            a = c;
            c = p;
          }
          this.referenceEditor.name = data[i].name;
          this.options2 = [];
          this.options2.push({
            value: data[i].src.shortName,
            label: data[i].src.shortName
          })
          this.options2.push({
            value: data[i].des.shortName,
            label: data[i].des.shortName
          })
          this.referenceEditor.initiator = data[i].init;
          this.referenceEditor.receiver = data[i].rec;
          this.referenceEditor.type = data[i].type;
          this.referenceEditor.constraint = data[i].cons;
          if (data[i].phe !== '') {
            this.referenceEditor.phe = data[i].phe;
          } else {
            this.referenceEditor.phe = '';
          }
          if (data[i].pheList.length !== 0) {
            this.referenceEditor.pheList = data[i].pheList;
          } else {
            this.referenceEditor.pheList = [];
          }
          if (y - Y <= 10 && Y - y <= 10 && x >= a && x <= c) {
            this.content = data[i];
            this.referenceEditor.dlgVisible = true;
            flag = 2;
            break;
          }
        } else if (data[i] instanceof ConstraintLine) {
          let a = data[i].x1;
          let b = data[i].y1;
          let c = data[i].x2;
          let d = data[i].y2;
          let k = (d - b) / (c - a);
          let m = b - k * a;
          let Y = k * x + m;
          // console.log(y, Y);
          if (a > c){
            let p = a;
            a = c;
            c = p;
          }
          this.constraintEditor.name = data[i].name;
          this.options3 = [];
          this.options3.push({
            value: data[i].src.shortName,
            label: data[i].src.shortName
          })
          this.options3.push({
            value: data[i].des.shortName,
            label: data[i].des.shortName
          })
          this.constraintEditor.initiator = data[i].init;
          this.constraintEditor.receiver = data[i].rec;
          this.constraintEditor.type = data[i].type;
          this.constraintEditor.constraint = data[i].cons;
          if (data[i].phe !== '') {
            this.constraintEditor.phe = data[i].phe;
          } else {
            this.constraintEditor.phe = '';
          }
          if (data[i].pheList.length !== 0) {
            this.constraintEditor.pheList = data[i].pheList;
          } else {
            this.constraintEditor.pheList = [];
          }
          if (y - Y <= 10 && Y - y <= 10 && x >= a && x <= c) {
            this.content = data[i];
            this.constraintEditor.dlgVisible = true;
            console.log(this.content.left, this.content.top);
            console.log(this.content.x1>this.content.x2? this.content.x2: this.content.x1);
            flag = 2;
            break;
          }
        }
      }
      let cb = this.canvas.getObjects();
      console.log(cb);
      if (flag === 1) {
        for (let i = 0; i < cb.length; i++) {
          let f = cb[i].getObjects()[0];
          if ((f instanceof fabric.Rect || f instanceof fabric.Ellipse) && cb[i].left === this.content.left && cb[i].top === this.content.top) {
            this.index = i;
            console.log("i:", i);
          }
        }
      }else if (flag === 2) {
        for (let i = 0; i < cb.length; i++) {
          let l = cb[i].getObjects()[0];
          if ((l instanceof fabric.Line || fabric.Path) && l.x1 === this.content.x1 && l.y1 === this.content.y1 && l.x2 === this.content.x2 && l.y2 === this.content.y2) {
            this.index = i;
            console.log("i:", i);
          }
        }
      }
    }.bind(this);
    this.canvas.on("mouse:dblclick", func2);
  }

  addMachine( x, y) {
    if(this.btn.machine){
      if (!this.machineDisabled) {
        this.machine = new Machine(x, y);
        this.machine.draw(this.canvas, this.canvasObj);
        this.canvasObj.addComponent(this.machine);
        this.machineDisabled = true;
      }
    }
  }
  addProblemDomain(x, y) {
    if(this.btn.problemDomain){

      let pD = new ProblemDomain(x, y);
      this.problemDomains.push(pD);
      pD.draw(this.canvas, this.canvasObj);
      this.canvasObj.addComponent(pD);
    }
  }
  addRequirement(x ,y) {
    if(this.btn.requirement){
      if (!this.requirementDisabled) {
        let rqm = new Requirement(x ,y);
        rqm.draw(this.canvas, this.canvasObj);
        this.canvasObj.addComponent(rqm);
        // this.requirementDisabled = true;
      }
    }
  }

  // Machine Editor Editor
  closeMEditor() {
    //cancel
    this.mEditor.dlgVisible = false;
    // this.mEditor.desc = "";
    // this.
  }
  closeMEditorAndSave() {
    //comfirm

    // this.machine.description = this.mEditor.desc;
    // this.machine.shortName = this.mEditor.sN;
    let data = this.canvas.getObjects();
    this.canvas.remove(data[this.index]);
    this.content.description = this.mEditor.desc;
    this.content.shortName = this.mEditor.sN;
    this.content.draw(this.canvas,this.canvasObj);
    this.mEditor.dlgVisible = false;
  }

  //Problem Domain Editor
  closePDEditor() {
    this.pDEditor.dlgVisible = false;
  }
  closePDEditorAndSave() {
    let data = this.canvas.getObjects();
    this.canvas.remove(data[this.index]);
    this.content.description = this.pDEditor.desc;
    this.content.shortName = this.pDEditor.sN;
    this.content.physicalPropety = this.pDEditor.pp;
    this.content.domainType = this.pDEditor.dT;
    this.content.draw(this.canvas, this.canvasObj);
    this.pDEditor.dlgVisible = false;
  }

  //Requirement Editor
  closeREditor() {
    //cancel
    this.rEditor.dlgVisible = false;
    this.rEditor.desc = "";
  }
  closeREditorAndSave() {
    //comfirm
    let data = this.canvas.getObjects();
    this.canvas.remove(data[this.index]);
    this.content.description = this.rEditor.desc;
    this.content.shortName = this.rEditor.sN;
    this.content.draw(this.canvas,this.canvasObj);
    this.rEditor.dlgVisible = false;
  }


  //interface Editor
  closeInterfaceEditor() {
    this.interfaceEditor.dlgVisible = false;
  }
  closeInterfaceAndSave(){
    let data = this.canvas.getObjects();
    this.canvas.remove(data[this.index]);
    //console.log(this.interaction)
    this.content.name = this.interfaceEditor.name;
    this.content.init = this.interfaceEditor.initiator;
    this.content.rec = this.interfaceEditor.receiver;
    this.content.phe = this.interfaceEditor.phe;
    this.content.type = this.interfaceEditor.type;
    this.content.pheList = this.interfaceEditor.pheList;
    //console.log(this.canvas)
    //console.log(this.canvasObject);
    this.content.draw(this.canvas);
    this.interfaceEditor.dlgVisible = false;

  }
  add1(){
    if(this.interfaceEditor.phe !== null){
      // console.log('xxxx')
      this.interfaceEditor.pheList.push(this.interfaceEditor.initiator+'! ' +this.interfaceEditor.phe+' '+this.interfaceEditor.type);
    }
  }

  // reference Editor
  openReferenceEditor(){
    this.referenceEditor.dlgVisible = true;
  }
  closeReferenceEditor(){
    this.referenceEditor.dlgVisible = false;
  }
  closeReferenceAndSave(){
    let data = this.canvas.getObjects();
    console.log("index:",this.index);
    this.canvas.remove(data[this.index]);
    this.content.name = this.referenceEditor.name;
    this.content.init = this.referenceEditor.initiator;
    this.content.rec = this.referenceEditor.receiver;
    this.content.phe = this.referenceEditor.phe;
    this.content.type = this.referenceEditor.type;
    this.content.cons = this.referenceEditor.constraint;
    this.content.pheList = this.referenceEditor.pheList;
    // console.log(this.content)
    this.content.draw(this.canvas);
    this.referenceEditor.dlgVisible = false;
  }
  add2() {
    if(this.referenceEditor.phe !== null){
      this.referenceEditor.pheList.push(this.referenceEditor.initiator+'! '+this.referenceEditor.phe+' '+this.referenceEditor.type)
    }
  }

  // constraint Editor
  openConstraintEditor(){
    this.constraintEditor.dlgVisible = true;
  }
  closeConstraintEditor(){
    this.constraintEditor.dlgVisible = false;
  }
  closeConstraintAndSave(){
    let data = this.canvas.getObjects();
    // console.log("index:",this.index)
    this.canvas.remove(data[this.index]);
    this.content.name = this.constraintEditor.name;
    this.content.init = this.constraintEditor.initiator;
    this.content.rec = this.constraintEditor.receiver;
    this.content.phe = this.constraintEditor.phe;
    this.content.type = this.constraintEditor.type;
    this.content.cons = this.constraintEditor.constraint;
    this.content.pheList = this.constraintEditor.pheList;
    // console.log(this.content)
    this.content.draw(this.canvas);
    this.constraintEditor.dlgVisible = false;
  }
  add3() {
    if(this.constraintEditor.phe !== null){
      this.constraintEditor.pheList.push(this.constraintEditor.receiver+'! '+this.constraintEditor.phe+' '+this.constraintEditor.type)
    }
  }


  stepBack() {
    if (this.step_active > -1) {
      switch(this.step_active){
        case 6: this.btn.constraint = true; this.btn.referenceLine = true; break;
        case 5: this.btn.constraint = false; this.btn.referenceLine = false; this.btn.requirement = true; break;
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
        case 5: this.btn.requirement = false; this.btn.constraint = true; this.btn.referenceLine = true; break;
        case 6: this.btn.constraint = false; this.btn.referenceLine = false; this.checkProblem(); break;
      }

    }
  }

  checkContext(){

    let mNum = 0;
    if (!this.canvas) {
      return;
    }
    let data = this.canvasObj.componentList;
    console.log(data);
    let flag = 1;
    for (let i = 0; i < data.length; i++) {
      if (data[i] instanceof InterfaceLine) {
        if (data[i].phe === ''){
          flag = 0;
        }
        if (data[i].pheList.length === 0) {
          flag = 0;
        }
      }
      if (data[i] instanceof Machine) {
        mNum++;
      }
      if (data[i] instanceof ProblemDomain) {
        if (data[i].has === false){
          flag = 0;
        }
      }
    }
    if (flag === 1 && data && mNum === 1) {
      this.isContextCompleted = true;
      alert("context Diagram is correct!");
    }
    else {
      this.isContextCompleted = false;
      this.step_active--;
      alert("context Diagram is incorrect! Please check it.")
    }
  }
  checkProblem(){
    if (!this.canvas) {
      return;
    }
    let data = this.canvasObj.componentList;
    console.log(data)
    let flag = 1;
    let rNum = 0;
    let iNum = 0;

    for (let i = 0; i < data.length; i++) {
      if (data[i] instanceof ReferenceLine) {
        if (data[i].phe === ''){
          flag = 0;
        }
        if (data[i].pheList.length === 0) {
          flag = 0;
        }
      }
      if (data[i] instanceof ConstraintLine) {
        iNum++;
        if (data[i].phe === ''){
          flag = 0;
        }
        if (data[i].pheList.length === 0) {
          flag = 0;
        }
      }
      if (data[i] instanceof Requirement) {
        rNum++;
        // if (rNum > 1){
        //   flag = 0;
        // }
      }
    }
    if (flag === 1 && data && iNum >= 1 && rNum >=1) {
      alert("Problem Diagram is correct!");
    }
    else {
      // this.step_active = 5;
      alert("Problem Diagram is incorrect! Pleasee check it.");
    }
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

class Canvas {
  componentList=[];

  draw(canvas)
  {
    let c = canvas.getObjects();
    for(let i=0;i<c.length;i++)
    {
      let g = c[i].getObjects();
      if(g[0] instanceof fabric.Line || g[0] instanceof fabric.Path)
      {
        canvas.remove(c[i]);
        canvas.renderAll();
      }
    }
    for(let i=0;i<this.componentList.length;i++)
    {
      if(this.componentList[i] instanceof Line)
      {
        //console.log("line");
        this.componentList[i].draw(canvas);
      }
    }
  }
  addComponent(component){
    this.componentList.push(component);
  }

}

class CanvasComponent {
  draw(canvas, canvasObj?){}
}

class Shape extends CanvasComponent {
  public description: string = "";
  public width = 150;
  public height = 70;

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
  // public width: number = 0;
  // public height: number = 0;


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
      width: this.width,
      height: this.height,
      strokeWidth: 3,
      stroke: "black"
    });
    let rect2 = new fabric.Rect({
      fill: "#ccc",
      left: 10,
      width: 10,
      height: this.height,
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
  }

  draw(canvas, canvasObject) {
    this.createGroup();
    canvas.add(this.machine);
    let func = function () {
      this.left = this.machine.left;
      this.top = this.machine.top;
      canvasObject.draw(canvas);
      // console.log(this.left, this.top);
    }.bind(this);
    this.machine.on("moving", func);
    this.machine.bringToFront();
    canvas.renderAll();
  }
}

class ProblemDomain extends Shape {
  public description: string = "ProblemDomain";
  public shortName: string = "PD";
  public physicalPropety: string = "GivenDomain";
  public domainType: string;
  public left;
  public top;
  public problemDomain: fabric.Group;
  public mark:string;

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
    switch(this.domainType){
      case "Causal": this.mark = "C";break;
      case "Biddable": this.mark = "B";break;
      case "Lexical": this.mark = "X";break;
      default: this.mark = "";
    }
    let rect = new fabric.Rect({
      fill: "#ccc",
      width: this.width,
      height: this.height,
      strokeWidth: 3,
      stroke: "black"
    });
    let rect2 = new fabric.Rect({
      fill: "#ccc",
      left: 0,
      width: 10,
      height: this.height,
      strokeWidth: 3,
      stroke: "black"
    });
    let text = new fabric.Text(this.description + "\n(" + this.shortName +")",{
      fontSize: 16,
      top: 20,
      left: 20,
      fill: "black",
      strokeWidth: 2,
      textAlign: "center"
    });
    let text2 = new fabric.Text(this.mark, {
      fontSize: 16,
      strokeWidth: 3,
      fill: "black",
      top: this.height - 20,
      left: this.width - 20
    })

    if(this.physicalPropety === "GivenDomain"){
      this.problemDomain = new fabric.Group([rect,text, text2],{
        top: this.top,
        left:this.left
      });
    }else if(this.physicalPropety === "DesignDomain"){
      this.problemDomain = new fabric.Group([rect,rect2,text, text2],{
        top: this.top,
        left:this.left
      });
    }

  }

  draw(canvas, canvasObject) {
    this.createGroup();
    canvas.add(this.problemDomain);
    let func = function () {
      this.left = this.problemDomain.left;
      this.top = this.problemDomain.top;
      canvasObject.draw(canvas);
      // console.log(this.left, this.top);
    }.bind(this);
    this.problemDomain.on("moving", func);
    this.problemDomain.bringToFront();
    canvas.renderAll();
  }
}

class Requirement extends Shape {
  public description = "Requirement";
  public shortName = "R";
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
      rx: this.width/2,
      ry: this.height/2,
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

  }

  // 18px的字宽为13
  draw(canvas, canvasObject) {
    this.createGroup();
    canvas.add(this.requirement);
    let func = function () {
      this.left = this.requirement.left;
      this.top = this.requirement.top;
      // console.log(this.left, this.top);
      canvasObject.draw(canvas);
    }.bind(this);
    this.requirement.on("moving", func);
    this.requirement.bringToFront();
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
  public init = "";
  public rec = "";
  public typeName = "Event";
  public phe ="";
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
      fontSize: 28,
      left: (x1+x2)/2,
      top: (y1+y2)/2,
      originX: 'center',
      originY: 'center',
      //preserveObjectStacking:true,
    });
    let group = new fabric.Group([line, text], {
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
      fontSize: 28,
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
  public top = 0;
  public left = 0;

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
    this.left =  (x1<=x2)?x1:x2;
    this.top = (y1<=y2)?y1:y2;
    let theta = 20;
    let healen = 20;
    // @ts-ignore
    let line = new fabric.Path(drawArrowBase(x1,y1,x2,y2,theta,healen),{
      fill:"black",
      stroke:"black",
      strokeWidth:2,
      // @ts-ignore
      x1:this.x1,
      // @ts-ignore
      x2:this.x2,
      // @ts-ignore
      y1:this.y1,
      // @ts-ignore
      y2:this.y2,
      //preserveObjectStacking:true,
      selectable:false,
      strokeDashArray:[10,5],
    });

    let text = new fabric.Text(this.name,{
      fontSize: 28,
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
