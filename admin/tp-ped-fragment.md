{% from "common/macros.njk" import embed_topic with context %}

#### <span class="badge bg-primary">PE-D</span> <span class="text-primary">Overview</span>

**What**: The latest release of the {{ version_penultimate }} period is subjected to a round of peer _acceptance/system testing_, also called the _Practical Exam (PE) Dry Run_ as this round of testing will be similar to the graded <trigger trigger="click" for="modal:projectDeliverablesPeDryRun-pe">Practical Exam that will be done at {{ version_final }}</trigger>.

**When, where**: uses a 40 minute slot at the start of week 11 lecture slot.
{% if cs2103 %}It will be conducted via Zoom. If you can't find another suitable location to join the Zoom meeting, you may do so from the lecture venue.{% endif%}

<modal large header="Admin {{ icon_embedding }} tP → Practical Exam" id="modal:projectDeliverablesPeDryRun-pe">
  <include src="tp-pe.md#tp-practicalexam"/>
</modal>

{{ icon_important_big_red }} **Grading**: The PE dry run ==affects your grade in the following ways==.
* You can earn ==up to 2 bonus marks== for your performance in the PE-D.{% if cs2103 %}<br>
 Graded ==based on the usefulness and the quality of your bug reports==, as evaluated by the receiving team.{% endif %}
* PE dry run is a way for you to practice for the actual PE.
* Taking part in the PE dry run will earn you participation points.
* There is ==no penalty for bugs reported== in your product. Every bug you find is a win-win for you and the team whose product you are testing.

**Why**:
* **To train you** to do manual testing, bug reporting, bug <tooltip content="assigning of priority order">triaging,</tooltip> bug fixing, communicating with users/testers/developers, evaluating products etc.
* **To help you improve your product** before the final submission.

#### <span class="badge bg-primary">PE-D</span> <span class="text-primary">Preparation</span>

<include src="tp-testing-fragment.md#testingPreparations" var-pe_active_tab="0"/>

#### <span class="badge bg-primary">PE-D</span> <span class="text-primary">During the session</span>

**Use ==the [CATcher Web app](https://catcher-org.github.io/CATcher/)==** for reporting bugs. More instructions will be given during the PE-D %%e.g., which session to use%%.

**Use MS Teams (not Zoom) to contact prof** if you need help during the session. Use Zoom chat only if you don't get a response via MS Teams.

<box type="warning" seamless>

****How many bugs to report?****

Report as many bugs as you can find during the given time. Take longer if you need (unlike the PE, PE-D is not timed strictly). If you can't find many bugs at this stage when the product is largely untested, you are unlikely to be able to find enough bugs in the better-tested final submission later. In that case, all the more reasons to spend more time and find more bugs now.

{% if cs2113 %}
**Insincere bug reports** will not count for credit %%i.e., don't submit 'fake' bug reports to increase the bug count.%%

**The median number of bugs reported in the previous semester's PE-D was 9.** Someone reporting just a 2-3 bugs is usually a sign of a half-hearted attempt rather than lack of bugs to find. If you really can't find bugs, at least submit suggestions for improvements.

{% elseif cs2103 %}
Evaluation rubric:

* Meets expectations: 3-5 good bug reports
* Exceeds expectations: more than 5 good bug reports

A _good_ bug report,

[a] has a descriptive title,<br>
[b] has enough details,<br>
[c] severity/type labels chosen are not too far off,<br>
[d] is written in a non-confrontational tone, and<br>
[e] points out a potentially problematic behavior (or a good way to improve the product)

As you can't be sure which of your bug reports will be considered as _good_ (criterion [e] in particular) by the receiving team, we recommend that you aim to submit at least 8-10 bug reports in total. The **median bug reports count in the last round was 9**.
{% endif %}
</box>

**PE and PE-D are _manual_ testing sessions.** Using test automation tools or scripting is not allowed.

**Test the product and report bugs** as described below, when the prof informs you to begin testing.

{% set pe_active_tab="0" %}

<div id="tp-pe-testing-instructions">

<box>

##### <div class="text-white bg-secondary p-1">**Testing instructions for PE and PE-D**</div>

###### <div class="text-white bg-secondary p-1">a) Launching the JAR file</div>

* Get the jar file to be tested:

<tabs active="{{ pe_active_tab }}">
<tab header="PE Dry Run (at **{{ version_penultimate }}**)">

* Download the _latest_ JAR file from the team's releases page, if you haven't done this already.

</tab>
<tab header="PE (at **{{ version_final }}**)">

* Download the zip file from the given location (to be given to you at least a few hours before the PE), if you haven't done that already.
* Unzip the downloaded zip file with the password (to be given to you at the start of the PE, via Canvas gradebook). This will give you another zip file with the name suffix `_inner.zip`.
* Unzip the inner zip file. This will give you the JAR file and other PDF files needed for the PE. ==Warning: do not run the JAR file while it is still inside the zip file==.<br>
  Ignore the `padding_file` found among the extracted files. %%Its only purpose is to mask the true size of the JAR file so that someone cannot guess which team they will be testing based on the zip file size.%%

</tab>
</tabs>
<p/>

* Put the JAR file ==in an empty folder== in which the app is allowed to create files %%(i.e., do not use a write-protected folder)%%.<br>
  In rare cases, the team could have submitted a ZIP file instead of a JAR file. In that case, unzip that file into the target folder.
* Open a command window. Run the `java -version` command to ensure you are using Java 11.<br>
  {{ icon_important_big_red }} Do this again even if you did this before, as your OS might have auto-updated the default Java version to a newer version.
* Check the UG to see if there are extra things you need to do before launching the JAR file %%e.g., download another file from somewhere%%<br>
  {{ icon_info }} You may visit the team's _releases_ page on GitHub if they have provided some extra files you need to download.
* Launch the jar file ==using the `java -jar` command== rather than double-clicking %%(reason: to ensure the jar file is using the same java version that you verified above)%%. Use double-clicking as a last resort.<br>
  {{ icon_info }} If the JAR file name has spaces in it, remember to surround it with double quotes.<br>
  e.g., `java -jar "Name With Spaces.jar"`<br>
  {{ icon_info }} Windows users: use the DOS prompt or the PowerShell (not the WSL terminal) to run the JAR file.{% if cs2103 %}<br>
  {{ icon_info }} Linux users: If the JAR fails with an error labelled `Gdk-CRITICAL` (happens in Wayland display servers), try running it using `GDK_BACKEND=x11 java -jar jar_file_name.jar` command instead.{% endif %}

<box type="info">

**++If the product doesn't work at all:++** If the product fails catastrophically %%e.g., cannot even launch, or even the basic commands crash the app%%, {% if not cs2103 %}contact the invigilator (via MS Teams, and failing that, via Zoom chat) to receive a _fallback_ team to test.{% else %}do the following:

1. **Check the UG** of the team, to see if there are extra things you need to do before launching the JAR.<br>
   **Confirm that you are using Java 11** and using the `java -jar` command to run the JAR, as explained in points above.
1. **Contact our head TA** via MS Teams (name: `Kim Hyeongcheol`, NUSNET: `dcskh`) and give him<br>
(a) **a screenshot** of the error message, and<br>
(b) **your GitHub** username.
1. **Wait for him to give you a fallback** team to test.<br>
   Contact the prof (via MS Teams) if you didn't get a response from Kim within 5 minutes.
1. **Delete bug reports you submitted for the previous team** (if any), using CATcher.
1. **You should not go back to testing the previous team** _after_ you've been given a fallback team to test.
{% endif %}
</box>


###### <div class="text-white bg-secondary p-1">b) What to test</div>

<tabs active="{{ pe_active_tab }}">
<tab header="PE Dry Run (at **{{ version_penultimate }}**)">

* Test the product ==based on the User Guide== available from their GitHub website `https://{team-id}.github.io/{{ tp_repo_name }}/UserGuide.html`.
* Do ==_system_ testing first== %%i.e., does the product work as specified by the documentation?%%. If there is time left, you can ==do _acceptance_ testing as well== %%i.e., does the product solve the problem it claims to solve?%%.

</tab>
<tab header="PE (at **{{ version_final }}**)">

* Test ==based on the Developer Guide== (Appendix named _Instructions for Manual Testing_) ==and the User Guide==. The testing instructions in the Developer Guide can provide you some guidance but if you follow those instructions strictly, you are unlikely to find many bugs. You can deviate from the instructions to probe areas that are more likely to have bugs.
* As before, do both ==_system testing_ and _acceptance testing_== but give priority to system testing as those bugs can earn you more credit.
* {{ icon_tip }} Be careful when copying commands from the UG (PDF version) to the software as some PDF viewers can affect the pasted text. If that happens, you might want to open the UG in a different PDF viewer.

</tab>
</tabs>
<p/>

###### <div class="text-white bg-secondary p-1">c) What bugs to report?</div>

<tabs active="{{ pe_active_tab }}">
<tab header="PE Dry Run (at **{{ version_penultimate }}**)">

* You may report functionality bugs, UG bugs, and feature flaws.
<div class="indented-level2">

{{ embed_topic("tp-grading-bugs-fragment.md#functionalityBugs", "Admin " + icon_embedding + " tP Grading → **Functionality Bugs**", "3") }}
{{ embed_topic("tp-grading-bugs-fragment.md#featureFlaws", "Admin " + icon_embedding + " tP Grading → **Feature Flaws**", "3") }}
{{ embed_topic("tp-grading-bugs-fragment.md#ugBugs", "Admin " + icon_embedding + " tP Grading → **Possible UG Bugs**", "3") }}
</div><p/>

* You can also post suggestions on how to improve the product.<br>
  {{ icon_tip }} Be diplomatic when reporting bugs or suggesting improvements. For example, instead of criticising the current behavior, simply suggest alternatives to consider.

</tab>
<tab header="PE (at **{{ version_final }}**)">

* Report functionality bugs:

{{ embed_topic("tp-grading-bugs-fragment.md#functionalityBugs", "Admin " + icon_embedding + " tP Grading → **Functionality Bugs**", "3", indent="2") }}


* Do not post suggestions but if the product is missing a critical functionality that makes the product less useful to the intended user, it can be reported as a bug of type `Type.FeatureFlaw`. The dev team is allowed to reject bug reports framed as mere suggestions or/and lacking in a convincing justification as to why the omission of that functionality is problematic.
{{ embed_topic("tp-grading-bugs-fragment.md#featureFlaws", "Admin " + icon_embedding + " tP Grading → **Feature Flaws**", "3", indent="2") }}

* You may also report UG bugs.
{{ embed_topic("tp-grading-bugs-fragment.md#ugBugs", "Admin " + icon_embedding + " tP Grading → **Possible UG Bugs**", "3", indent="2") }}


</tab>
</tabs>
<p/>


###### <div class="text-white bg-secondary p-1">d) How to report bugs</div>

* {{ icon_important_big_red }} **Post bugs as you find them** %%(i.e., do not wait to post all bugs at the end)%% because <span class="text-danger">bug reports created/modified after the allocated time will not count.</span>
<div class="indented-level2">

<tabs>
<tab header="Using CATcher">

* Launch CATcher, and login to the correct profile (when CATcher asks, consent to creating a new repo):
  * PE Dry Run: `{{ module_pair }} PE Dry run`
  * PE: `{{ module_pair }} PE`
* Post bugs using CATcher.
</tab>
<tab header="Not using CATcher">
<div class="indented-less">

<include src="tp-testing-fragment.md#not-using-catcher-warning" />

<markdown>
* Post bug reports in the following repo you created earlier:
  * PE Dry Run: `ped`
  * PE: `pe`
* The whole description of the bug should be in the issue description i.e., ==do not add comments to the issue==.
</markdown>
</div>
</tab>
</tabs>
</div>

###### <div class="text-white bg-secondary p-1">e) Bug report format</div>

* Each bug should be a separate issue %%i.e., do not report multiple problems in the same bug report.%%<br>
  If there are multiple bugs in the same report, the dev team will select only one of the bugs in the report and discard the others.
* When reporting similar bugs, it is safer to report them as separate bugs because there is no penalty for reporting duplicates while putting multiple bugs in the same report can reduce your bug count (see the previous point). But as submitting multiple bug reports take extra time, if you are quite sure they will be considered as _duplicates_ by the dev team later, you can report them together, to save time.
* Write good quality bug reports; ==poor quality or incorrect bug reports will not earn credit==.
* Use a descriptive title.
* Give a good description of the bug with ==steps to reproduce, expected, actual, and screenshots==. If the receiving team cannot reproduce the bug, you will not be able to get credit for it.
* Assign exactly one `severity.*` label to the bug report. Bug reports without a severity label are considered `severity.Low` (lower severity bugs earn lower credit)<br>

<div class="indented-level2">
<include src="appendixE-gitHub.md#bug-severity" />
</div>

* Assign exactly one `type.*` label to the issue.
<div class="indented-level2">
<include src="tp-pe-fragment.md#type-labels" />
</div>


* {{ icon_info }} **If you need to include `<` or `>` symbols in your bug report**, use HTML equivalents for 'less than'
and 'greater than' instead (i.e., `&lt;` and `&gt;` e.g., `x &lt; y` instead of `x < y`).<br>
Reason: CATcher and GitHub strips out content wrapped in `<` and `>`, for security reasons.
</box>
</div>

<modal large header="Admin {{ icon_embedding }} Project →" id="modal:v1.3-ppp">
  <include src="tp-deliverables.md#tp-deliverables-ppp"/>
</modal>

<span id="after-ped">

#### <span class="badge bg-primary">PE-D</span> <span class="text-primary">After the session</span>
* **The relevant bug reports will be transferred to your issue tracker** within a day after the session is over. Once you have received the bug reports for your product, you can decide whether you will act on reported issues before the final submission {{ version_final }}. For some issues, the correct decision could be to reject or postpone to a version beyond {{ version_final }}.<br>
  Reminder: There is no penalty for any of the bugs you received in the PE-D.

<div class="indented-level2">

<box type="info" seamless icon=":fas-angry:">

****Dealing with "What the h___ the tester was thinking?" type bug reports****{.text-info}

Some bug reports will make you angry because they seem baseless, wrong, rude etc. It's still possible to get value from such bug reports though:
* After you got over the initial indignation, dig deeper to see if there's even the slightest possibility that there is a bug. For example, consider this scenario:
   1. The tester claims a certain command doesn't work.
   1. All your team members tried the exact same command and it works as advertised. What the h___ the tester is trying to pull here?
   1. In reality, the error is actually caused by a duplicate entry in the database resulting from a previous command; the tester didn't mention that command in the bug report (because s/he didn't realize the two are connected).
* The reported bug might be non-existent but the tester's screw up can indicate other areas to improve. For example, the tester reports a missing feature that is clearly mentioned as 'not implemented' in the UG, but perhaps the UG can be improved to make that fact harder to miss?
* What exactly about the bug report that makes you angry? Remind yourself not to do the same offence when you report bugs yourself in future.

Use the pain of dealing with this kind of bug reports as an opportunity to develop the following mindset:

**(a) The product is _guilty until proven innocent_**: If the bug report has even a _hint_ of something amiss with the product, it's your (not the tester's) responsibility to try and prove if it is really a problem or not. Why? because finding a bug is a win for _you_ -- as you can then fix it and thereby avoid the embarrassment of releasing a buggy product.

**(b) A crappy bug report is better than no bug report**: If the bug actually exists, it is better to have _some_ indication about it than none at all. In a real project, a tester that fails to find bugs can cause more harm to your career than a tester who finds bugs but doesn't report them well.

**(c) I _used_ to get angry at bug reports, but not any more**: If you work hard, take pride in the quality of your work, it's no wonder that you get angry when others find faults with your work incorrectly. Aim to move past that phase where you take bug reports personally. The sooner you can tackle any sh*tty bug report calmly and objectively, the sooner you'll rise to the 'professional' software engineer level.
</box>
</div>


* **If you have received stray bug reports** (i.e., bug reports that don't seem to be about your project), do let us know ASAP (email the prof).
* **You can navigate to the original bug report** (via the back-link provided in the bug report given to you) and post in that issue thread to communicate with the tester who reported the bug %%e.g. to ask for more info%%, etc. However, the tester is not obliged to respond. Note that simply replying to the bug report in your own repo will not notify the tester.
  * {{ icon_tip }} Do not argue with the tester to try to convince that person that your way is correct/better. If at all, you can gently explain the rationale for the current behavior but do not waste time getting involved in long arguments. If you think the suggestion/bug is unreasonable, just thank the tester for their view and discontinue to discussion.
* **Aim to ==do a systematic triaging of issues received==**. Some suggestions:
  * Close duplicate issues.
  * Use labels (create new labels if necessary) to,
    * differentiate _bugs_ from the rest (e.g., feature suggestions/flaws).
    * indicate priority of the bugs that need fixing.
  * Assign each bug to the person who should fix it.
* **You may ignore `type/severity.*` labels given by the tester.** They will not affect you or the tester either way -- they were there just for the testers to practice. You may apply your own type/severity labels if you wish.<br>
  {{ icon_important_big_red }} In particular, beware of simply following the `type.*` given by the tester; it is your job to decide the correct type of the issue. e.g., What the tester labeled as a _bug_ might actually be a _feature flaw_.{% if cs2103 or tic4002 %}
* **If a bug report is simply a feature suggestion**, you can take note of it and close it (to reduce clutter in the issue tracker, and to make it easy for the teaching team to track your progress on dealing with PE-D issues). Similarly, you can close PE-D issues not relevant to {{ version_penultimate }}.{% endif %}


<box type="wrong" seamless>

**Note that listing bugs as 'known bugs' in the UG or specifying _unreasonable_ constraints in the UG to make bugs 'out of scope' will not exempt those bugs from the final grading.** That is, PE testers can still earn credit for reporting those bugs and you will still be penalized for them.<br>
 However, a product is allowed to have 'known limitations' %%(e.g., a daily expense tracking application meant for students is unable to handle expenses larger than $999)%% as long as they don't degrade the product's use within the intended scope. They will not be penalized.
</box>
<box type="info" tags="m--cs2103 m--tic4002" seamless>

**Even bugs inherited from AB3 need to be fixed.** As mentioned in a previous week, even bugs you inherited from AB3 need fixing %%(because "we inherited it from the previous dev team" is not a valid excuse to leave a bug unfixed)%%. If you are unsure if something is such a bug that need fixing, please post in the forum.
</box>
<box type="tip" seamless>

**Identify bugs you missed in the PE-D:** Visit the issue tracker of the team you tested and see bugs reported by others who tested the same product. Identify bugs you missed (if any). That knowledge might help you find similar bugs in your own product as well as find more bugs during the PE.
</box>
</span>
