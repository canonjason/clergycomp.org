<?php
$number_format = "number_format";
if (isset($_POST["valuea"])) $valuea = $_POST["valuea"]; //salary
if (isset($_POST["valueb"])) $valueb = $_POST["valueb"]; //rectory frv
if (isset($_POST["valuec"])) $valuec = $_POST["valuec"]; //cash housing
if (isset($_POST["valued"])) $valued = $_POST["valued"]; //cash utilities
if (isset($_POST["valuee"])) $valuee = $_POST["valuee"]; //utilities paid by church
if (isset($_POST["valuef"])) $valuef = $_POST["valuef"]; //custodial acct
if (isset($_POST["valueg"])) $valueg = $_POST["valueg"]; //medical
if (isset($_POST["valueh"])) $valueh = $_POST["valueh"]; //dental
if (isset($_POST["valuei"])) $valuei = $_POST["valuei"]; //travel
if (isset($_POST["valuej"])) $valuej = $_POST["valuej"]; //con ed

if (($valuec + $valued) > $valueb) {
  $error = "<b>!</b>&nbsp;&nbsp;<i>Cash housing/utilities cannont Subtotal more than FRV of provided housing.</i>";
  $error1 = "<b>!</b>";
}

$life = 0;
$cash_house_utl = $valuec + $valued;

$answer1 = ($valuea / 1.0765) - $valueb; //stipend
$answer2 = ($answer1 + $valueb) * .0765; //seca
$answer2_1 = $answer1 + $valuec + $valued + $answer2; //Subtotal cash comp (not equity all.)
$answer2_2 = $valuef + $answer2_1; //Subtotal cash and custodial account contribution
$answer3 = $answer1 + $valueb + $answer2; //Subtotal cash and non cash compensation
$answer3_1 = (($answer2_2 + $valuee) * .3) + ($answer2_2 + $valuee); //pension base
$answer3_2 = $answer3_1 * .18; //pension assessment
$answer6 = $valueg + $valueh +$life; //subSubtotal insurance
$answer7 = $valuei + $valuej; //subSubtotal reimbursables
$answer8 = $answer2_2 + $answer3_2 + $answer6 + $answer7 + $valuee; //Subtotal cash expense
$answer9 = $valueb - ($valuec + $valued);
$answer10 = $answer2_2 + $valuee; //cash, custodial, utilities paid by church
$answer11 = ($answer2_2 + $valuee) * .3;
$answer12 = $answer2_1 / 24; //twice-monthly gross

echo  <<<_END
<center>
<form method="post" action="/calculators/clergy_ws_ph.php">
<table border="0" width="700px" cellpadding="3" cellspacing="1" class="table">

<!--<tr><td>&nbsp;</td><td><img src="logo2.png" height="20" style="padding-bottom:5px;"></td></tr>-->
<tr class="calcheading"><td>&nbsp;</td><td colspan="4"><strong>Clergy Compensation Worksheet</strong></td><td>&nbsp;</td><td>&nbsp;</td></tr>

<tr class="calcheading"><td>&nbsp;</td><td colspan="4"><i>Church-Provided Housing</i></td><td>&nbsp;</td><td>&nbsp;</td></tr>

<tr><td>&nbsp;</td><td>&nbsp;</td><td>&nbsp;</td><td></td></tr>
<tr><td>&nbsp;</td><td colspan="3">$error</td><td>&nbsp;</td><td></td></tr>
<tr><td colspan="3"><hr></td><td>&nbsp;</td><td>&nbsp;</td></tr>

<tr class="calcrow"><td>1</td><td><b>Salary</b>:<b><sup>1</sup></b></td><td align="right"><input type="text" name="valuea" value="$valuea" style="text-align:right;"/></td><td></td></tr>
<tr class="calcrow"><td>2</td><td><b>Fair Rental Value of Provided Housing:</b><b><sup>2</sup></b></td><td align="right"><input type="text" name="valueb" value="$valueb" style="text-align:right;"/></td><td></td></tr>
<tr class="calcrow"><td>3</td><td><b>Cash Housing Allowance</b> <font size="-1"><i>(a portion of FRV)</i></font>:<b><sup>3</sup></b></td><td align="right"><input type="text" name="valuec" value="$valuec" style="text-align:right;"/></td><td>$error1</td></tr>
<tr class="calcrow"><td>4</td><td><b>Cash Utilities Allowance</b> <font size="-1"><i>(a portion of FRV)</i></font>:</td><td align="right"><input type="text" name="valued" value="$valued" style="text-align:right;"/></td><td>$error1</td></tr>
<tr class="calcrow"><td>5</td><td><b>Utilites Paid by the Church:</b><b><sup>4</sup></b></td><td align="right"><input type="text" name="valuee" value="$valuee" style="text-align:right;"/></td><td></td></tr>
<tr><td colspan="3"><hr></td><td>&nbsp;</td><td>&nbsp;</td></tr>

<tr class="calcrow"><td>6</td><td>Cash Stipend:</td><td align="right">{$number_format($answer1,2)}</td><td>&nbsp;</td></tr>
<!--<tr class="calcrow"><td>8</td><td>Non-Cash Compensation:</td><td align="right">{$number_format($answer9,2)}</td><td>&nbsp;</td></tr>-->
<tr class="calcrow"><td>7</td><td>Cash Housing/Utilities Allowance:</td><td align="right">{$number_format($cash_house_utl,2)}</td><td>&nbsp;</td></tr>
<tr class="calcrow"><td>8</td><td>Self-Employment Tax Reimbursement <font size="-1"><i>(SECA)</i></font>:<b><sup>5</sup></b></td><td align="right">{$number_format($answer2,2)}</td><td>&nbsp;</td></tr>
<tr class="calcrow"><td>9</td><td>Equity Allowance:<b><sup>6</sup></b></td><td align="right"><input type="text" name="valuef" value="$valuef" style="text-align:right;"/></td><td></td></tr>
<!--<tr class="calcrow"><td>10</td><td>Cash & Non-Cash Compensation (salary):</td><td align="right">{$number_format($answer3,2)}</td><td>&nbsp;</td></tr>-->
<tr class="calcrow"><td>10</td><td><b>Subtotal Cash Compensation:</b><b><sup>7</sup></b></td><td align="right"><b>{$number_format($answer2_2,2)}</b></td><td>&nbsp;</td></tr>
<!--<tr class="calcrow"><td>11</td><td><b>Subtotal Cash and Custodial Account Contribution:</b></td><td align="right">{$number_format($answer2_2,2)}</td><td>&nbsp;</td></tr>-->
<tr><td>&nbsp;</td><td>&nbsp;</td><td>&nbsp;</td></tr>

<!--<tr class="calcrow"><td>11</td><td>Pension Base:<b><sup>8</sup></b></td><td align="right"><b>{$number_format($answer3_1,2)}</b></td><td></td></tr>-->
<tr class="calcrow"><td>11</td><td><b>Pension Assessment:</b><b><sup>8</sup></b></td><td align="right"><b>{$number_format($answer3_2,2)}</b></td><td>&nbsp;</td></tr>
<tr><td>&nbsp;</td><td>&nbsp;</td><td>&nbsp;</td></tr>

<tr class="calcrow"><td>12</td><td>Medical Insurance Premium:<b><sup>9</sup></b></td><td align="right"><input type='text' name='valueg' value="$valueg" style="text-align:right;"/></td><td></td></tr>
<tr class="calcrow"><td>13</td><td>Dental Insurance Premium:</td><td align="right"><input type='text' name='valueh' value="$valueh" style="text-align:right;"/></td><td></td></tr>
<tr class="calcrow"><td>14</td><td>$40,000 Group Term Life Insurance:</td><td align="right">{$number_format($life,2)}</td><td>&nbsp;</td></tr>
<tr class="calcrow"><td>15</td><td><b>Subtotal Insurance Benefits:</b></td><td align="right"><b>{$number_format($answer6,2)}</b></td><td>&nbsp;</td></tr>
<tr><td>&nbsp;</td><td>&nbsp;</td><td>&nbsp;</td></tr>

<tr class="calcrow"><td>16</td><td>Travel/Business Expense Reimbursement:<b><sup>10</sup></b></td><td align="right"><input type='text' name='valuei' value="$valuei" style="text-align:right;"/></td><td></td></tr>
<tr class="calcrow"><td>17</td><td>Continuing Education Reimbursement:</td><td align="right"><input type='text' name='valuej' value="$valuej" style="text-align:right;"/></td><td></td></tr>
<tr class="calcrow"><td>18</td><td><b>Subtotal Reimburseable Expenses:</b></td><td align="right"><b>{$number_format($answer7,2)}</b></td><td>&nbsp;</td></tr>
<tr><td>&nbsp;</td><td>&nbsp;</td><td>&nbsp;</td></tr>

<tr class="calcrow"><td>19</td><td><b>Total Package</b> <font size="-1"><i>(excluding non-cash compensation)</i></font>:</td><td align="right"><b>{$number_format($answer8,2)}</b></td><td>&nbsp;</td></tr>
<tr><td colspan="3"><hr></td><td>&nbsp;</td><td>&nbsp;</td></tr>

<tr><td>&nbsp;</td><td><input type="submit" value="Calculate"/>&nbsp;<input type="button" value="Reset Form" onClick="window.location.href=window.location.href"></td><td>&nbsp;</td><td>&nbsp;</td></tr>
<tr><td colspan="3"><hr></td><td>&nbsp;</td><td>&nbsp;</td></tr>
<tr><td>&nbsp;</td><td>&nbsp;</td><td>&nbsp;</td></tr>
<tr><td>&nbsp;</td><td>&nbsp;</td><td>&nbsp;</td></tr>

<tr><td>&nbsp;</td><td colspan="3"><b><sup>1</sup></b> <i>A cleric’s salary is comprised of three figures: a cash stipend, a cash housing/utilities allowance (plus a non-cash housing amount if housing is provided), and, customarily, a self-employment tax reimbursement. When a position is advertised in the church, the salary is the principal figure.</i></td></tr>

<tr><td>&nbsp;</td><td colspan="3"><b><sup>2</sup></b> <i>Enlist the services of a local real estate broker to determine the estimated fair rental value of provided housing. This estimate should be based upon property location, size, condition, and include consideration for utilities, taxes, furnishings, and insurance.</i></td></tr>

<tr><td>&nbsp;</td><td colspan="3"><b><sup>3</sup></b> <i>A cash housing allowance is an optional way of handling a portion of the provided housing’s estimated fair rental value. A church may choose to pay the cleric an amount to offset direct purchases he or she makes for home maintenance, ie., light bulbs. A church may also pay the cleric an estimated amount for his or her utility costs. Housing and utilities costs, up to the estimated allowance amounts, are tax deductible for the cleric. Note that by adding a cash housing/utilties allowance, the non-cash compensation amount is decreased. In this case, with a cash housing/utilties total of <b>{$number_format($cash_house_utl,2)}</b>, the non-cash compensation amount becomes <b>{$number_format($answer9,2)}</b>. A vestry must approve the designation of a housing allowance. Download the <a href='../wp-content/uploads/2010/06/IRS-Housing-Allowance-Regulation-FRV-Resolution-Forms.pdf' target="blank">IRS Housing Allowance Regulation and applicable forms.</a></i></td></tr>

<tr><td>&nbsp;</td><td colspan="3"><b><sup>4</sup></b> <i>A church may choose to pay the provided housing's utility costs directly rather than, or in addition to, giving the cleric a utilities allowance.</i></td></tr>

<tr><td>&nbsp;</td><td colspan="3"><b><sup>5</sup></b> <i>The IRS considers clergy to be "self-employed," and therefore responible for paying SECA (Self-Employment Contributions Act) which comprises both the employee and employer portion of FICA (Federal Insurance Contributions Act). This 15.3% tax funds Social Security and Medicare. It has become the custom in the Episcopal Church for churches to reimburse their clergy half of the SECA tax (7.65%), effectively treating them as employees rather than self-employed independent contractors. It is important to note that a SECA reimbursment should not be calculated as an addition to the salary but, rather, as part of the salary. This worksheet "backs out" the appopriate SECA amount from the salary.</i></td></tr>

<tr><td>&nbsp;</td><td colspan="3"><b><sup>6</sup></b> <i>Since a cleric living in provided housing does not have the opportunity to build real estate equity, an equity allowance may help compensate. Equity allowances are commonly paid as pre-tax contributions to a 403b, IRA or another retirement account of the cleric's choosing. Although an equity allowance is deffered compensation, it is still considered part of the cash compensation and is a factor in calculating pension. It is not considered part of the salary.</i></td></tr>

<tr><td>&nbsp;</td><td colspan="3"><b><sup>7</sup></b> <i>The cleric's twice-monthly gross payroll amount can be determined by dividing the total cash compensation (excluding the equity allowance) by 24. In this case: <b>{$number_format($answer2_1,2)} ÷ 24 = {$number_format($answer12,2)}</b></i></td></tr>

<tr><td>&nbsp;</td><td colspan="3"><b><sup>8</sup></b> <i>When provided housing is invovled, the pension assessment is determined by first calculating the pension base. This is done by multiplying the sum of the cash compensation (including utilities paid by the church and deferred compensation, such as an equity allowance) by 30% and then adding the previous sum to the result. In this case, <b>{$number_format($answer10,2)} x 30% = {$number_format($answer11,2)}, then {$number_format($answer11,2)} + {$number_format($answer10,2)} = {$number_format($answer3_1,2)}.</b> The pension assessment is then calculated by multiplying the pension base by 18%: <b>{$number_format($answer3_1,2)} x 18% = {$number_format($answer3_2,2)}</b></i>.</td></tr>

<tr><td>&nbsp;</td><td colspan="3"><b><sup>9</sup></b> <i>The costs for medical and dental insurance premiums may be paid in full by the church or shared between the church and cleric, depending on the lay/clergy insurance parity arrangement determined by the vestry. 

<tr><td>&nbsp;</td><td colspan="3"><b><sup>10</sup></b> <i>It is customary to budget an amount clergy may be reimbursed from when incurring expenses in the course of professional activities on behalf of the church. Additionally, as the Canons of The Episcopal Church direct clergy to engage in continuing education, a reimbursement amount is also customarily budgeted for this purpose. The recommended annual amount for a full-time cleric is $2000 for each account. </i></td></tr>

</table>
</form>
</center>
_END;
?>