Download Link: https://assignmentchef.com/product/solved-ecs154b-lab4-pipelined-mips-cpu
<br>
<ul>

 <li>Build and test a pipelined MIPS CPU that implements a subset of the MIPS instruction set.</li>

 <li>Handle hazards through forwarding and stalling.</li>

</ul>

<h1>Description</h1>

In this lab, you will use Logisim to design and test a pipelined MIPS CPU that implements a subset of the MIPS instruction set, along with data hazard resolution. Make sure to use the given circuit for this assignment, as the Register File included implements internal forwarding, while previous versions do not.

<h1>Details</h1>

Your CPU must implement all instructions from Lab 3. Specifically, you must implement:

AND, ANDI, ADD, ADDI, OR, ORI, SUB, SLT, SLTU*, XOR, LW, SW, BEQ, J, JAL, JR, SLL, SRL

<strong>As with the previous labs, the instructions for SLTU are incorrect in the given MIPS PDF.</strong>

The instructions should read:

If rs <em>&lt; </em>rt with an unsigned comparison, put 1 into rd. Otherwise put 0 into rd.

The control signals for the ALU are identical to Lab 3, and are reposted here for your convenience:

<table width="354">

 <tbody>

  <tr>

   <td width="75">Operation</td>

   <td width="70">ALUCtl3</td>

   <td width="70">ALUCtl2</td>

   <td width="70">ALUCtl1</td>

   <td width="70">ALUCtl0</td>

  </tr>

  <tr>

   <td width="75">OR</td>

   <td width="70">0</td>

   <td width="70">0</td>

   <td width="70">0</td>

   <td width="70">0</td>

  </tr>

  <tr>

   <td width="75">SLTU</td>

   <td width="70">0</td>

   <td width="70">0</td>

   <td width="70">0</td>

   <td width="70">1</td>

  </tr>

  <tr>

   <td width="75">SLT</td>

   <td width="70">0</td>

   <td width="70">0</td>

   <td width="70">1</td>

   <td width="70">0</td>

  </tr>

  <tr>

   <td width="75">ADD</td>

   <td width="70">0</td>

   <td width="70">0</td>

   <td width="70">1</td>

   <td width="70">1</td>

  </tr>

  <tr>

   <td width="75">SUB</td>

   <td width="70">0</td>

   <td width="70">1</td>

   <td width="70">0</td>

   <td width="70">0</td>

  </tr>

  <tr>

   <td width="75">XOR</td>

   <td width="70">0</td>

   <td width="70">1</td>

   <td width="70">0</td>

   <td width="70">1</td>

  </tr>

  <tr>

   <td width="75">AND</td>

   <td width="70">0</td>

   <td width="70">1</td>

   <td width="70">1</td>

   <td width="70">0</td>

  </tr>

  <tr>

   <td colspan="5" width="354">…</td>

  </tr>

  <tr>

   <td width="75">SLR</td>

   <td width="70">1</td>

   <td width="70">1</td>

   <td width="70">1</td>

   <td width="70">0</td>

  </tr>

  <tr>

   <td width="75">SLL</td>

   <td width="70">1</td>

   <td width="70">1</td>

   <td width="70">1</td>

   <td width="70">1</td>

  </tr>

 </tbody>

</table>

Your CPU should not have any branch delay slots, and should use a branch not taken strategy for branch prediction. You may implement your control signals using any method you prefer. You can use combinational logic, microcode, or a combination of the two.

<h1>Hazards</h1>

As you have learned in lecture, pipelining a CPU introduces the possibilities of hazards. Your CPU must be able to handle <strong>all possible </strong>hazards. Your CPU must use forwarding where possible, and resort to stalling only where necessary. Below is a subset of the possible hazards you may encounter. This means that, while all possible hazards may not be listed here, your CPU must still be able to handle all possible hazards.

<ol>

 <li><strong>Read After Write </strong>(RAW) hazards. Your CPU must perform forwarding on both ALU inputs to prevent Read After Write hazards. Your CPU must handle the hazards in the following code segments without stalling.</li>

</ol>

<table width="470">

 <tbody>

  <tr>

   <td width="114">ADD $4, $5, $6</td>

   <td width="121">ADD $4, $5, $6</td>

   <td width="121">ADD $4, $5, $6</td>

   <td width="114">ADD $4, $5, $6</td>

  </tr>

  <tr>

   <td width="114">ADD $7, $4, $4</td>

   <td width="121">ADD $8, $9, $10</td>

   <td width="121">ADD $8, $9, $10</td>

   <td width="114">LW $8, 0($4)</td>

  </tr>

  <tr>

   <td width="114"> </td>

   <td width="121">ADD $7, $4, $4</td>

   <td width="121">ADD $7, $4, $8</td>

   <td width="114"> </td>

  </tr>

 </tbody>

</table>

<ol start="2">

 <li><strong>Load Use </strong> Your CPU must handle load-use hazards through stalling and forwarding. You may only stall when necessary. If you stall when forwarding would work, you will lose points.</li>

</ol>

<table width="242">

 <tbody>

  <tr>

   <td width="121">LW $8, 0($4)</td>

   <td width="121">LW $8, 0($4)</td>

  </tr>

  <tr>

   <td width="121">ADD $10, $9, $8</td>

   <td width="121">ADD $4, $5, $6</td>

  </tr>

  <tr>

   <td width="121"> </td>

   <td width="121">ADD $10, $9, $8</td>

  </tr>

 </tbody>

</table>

<ol start="3">

 <li><strong>Store Word </strong>(SW) hazards. Your CPU must handle all Read After Write hazards associated with SW using forwarding. You may need to stall in certain cases, as well.</li>

</ol>

<table width="328">

 <tbody>

  <tr>

   <td width="114">ADD $4, $5, $6</td>

   <td width="107">LW $4, 0($0)</td>

   <td width="107">LW $4, 0($0)</td>

  </tr>

  <tr>

   <td width="114">SW $4, 0($4)</td>

   <td width="107">SW $4, 10($0)</td>

   <td width="107">SW $5, 10($4)</td>

  </tr>

 </tbody>

</table>

<ol start="4">

 <li><strong>Control Flow </strong> Read After Write hazards can also occur with the BEQ and JR instructions.</li>

</ol>

The following hazards must be solved with forwarding.

<table width="589">

 <tbody>

  <tr>

   <td width="170">ADD $4, $5, $6</td>

   <td width="121">ADD $4, $5, $6</td>

   <td width="177">LW $10, 0($0)</td>

   <td width="121">LW $10, 0($0)</td>

  </tr>

  <tr>

   <td width="170">ADD $8, $9, $10</td>

   <td width="121">ADD $8, $9, $10</td>

   <td width="177">ADD $4, $5, $6</td>

   <td width="121">ADD $4, $5, $6</td>

  </tr>

  <tr>

   <td width="170">BEQ $0, $4, BranchAddr</td>

   <td width="121">JR $4</td>

   <td width="177">ADD $8, $9, $10</td>

   <td width="121">ADD $8, $9, $10</td>

  </tr>

  <tr>

   <td width="170"> </td>

   <td width="121"> </td>

   <td width="177">BEQ $0, $10, BranchAddr</td>

   <td width="121">JR $10</td>

  </tr>

 </tbody>

</table>

The following hazards should be resolved with stalls.

<table width="461">

 <tbody>

  <tr>

   <td colspan="3" width="170">ADD $4, $5, $6</td>

   <td width="114">ADD $4, $5, $6</td>

   <td colspan="3" width="177">LW $10, 0($0)</td>

  </tr>

  <tr>

   <td colspan="3" width="170">BEQ $0, $4, BranchAddr</td>

   <td width="114">JR $4</td>

   <td colspan="3" width="177">ADD $4, $5, $6</td>

  </tr>

  <tr>

   <td colspan="3" width="170"> </td>

   <td width="114"> </td>

   <td colspan="3" width="177">BEQ $0, $10, BranchAddr</td>

  </tr>

  <tr>

   <td width="31"> </td>

   <td width="114">LW $10, 0($0)</td>

   <td colspan="3" width="177">LW $10, 0($0)</td>

   <td width="107">LW $10, 0($0)</td>

   <td width="31"> </td>

  </tr>

  <tr>

   <td width="31"> </td>

   <td width="114">ADD $4, $5, $6</td>

   <td colspan="3" width="177">BEQ $0, $10, BranchAddr</td>

   <td width="107">JR $10</td>

   <td width="31"> </td>

  </tr>

  <tr>

   <td width="31"> </td>

   <td width="114">JR $10</td>

   <td colspan="3" width="177"> </td>

   <td width="107"> </td>

   <td width="31"> </td>

  </tr>

  <tr>

   <td width="31"></td>

   <td width="114"></td>

   <td width="24"></td>

   <td width="114"></td>

   <td width="38"></td>

   <td width="107"></td>

   <td width="31"></td>

  </tr>

 </tbody>

</table>

<h1>Branch Prediction</h1>

In order to reduce the number of stall cycles in our CPU, we will be using a branch not taken prediction strategy. This means that, if a branch is taken, we will need to provide hardware to squash the incorrectly predicted instructions. For example:

<table width="391">

 <tbody>

  <tr>

   <td width="170">BEQ $0, $0, BranchAddr</td>

   <td width="221"> </td>

  </tr>

  <tr>

   <td width="170">ADD $1, $1, $1</td>

   <td width="221">This instruction must be squashed.</td>

  </tr>

 </tbody>

</table>

The number of instructions that must be squashed is dependent on where in the pipeline you evaluate your branch condition. Jump instructions can be viewed as branches that are always taken and therefore are able to have their “branch” conditions evaluated in the Decode stage.