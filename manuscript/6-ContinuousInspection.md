# Continuous Inspection Throughout the Roadmap

![Continuous Inspection - Ensure what is fixed will remain fixed](\images\roadmap-ci.png)

## Why continuous inspection is important?

< under development >

## Which conventions should be put under continuous inspection?

< under development >

## Example: Using Jenkins and ConQAT to enable continuous clone detection

ConQAT has an excellent engine for detecting code clones (duplicates). Jenkins, on the other hand, can automatically run external tools and continuously check for code quality. If we can merge both, this will become an invaluable tool for technical teams and will save them a lot of review effort.

#### Step 1: Define Your Policy of Preventing Code Clones

This is done by specifying the ConQat Run configuration files (.cqr) using eclipse. That is, create a cqr file which detects kinds of duplicates that you don't want to see in the code altogether.

For example, you may define a cqr file which detects exact code clones of length greater than 10 lines of code. This is a screen shot of a cqr file which does exactly this job:

![ConQat "Run conf" file with parameters](\images\cqr.png)

#### Step 2: Invoke ConQAT cqr from Jenkins

Ant can do this using shell scripts or command lines, as follows:

> `cd  <ConQat Location>\conqat\binconqat -f <cqr file location>\JavaCloneAnalysis.cqr`

 Note: Make sure that you specify the output directory in the configuration file appropriately, because Jenkins will be accessing it later in the next step

#### Step3: Introspect the clones.xml file, and fail the build if it has clones

Let Ant check the clones.xml file generated. This is an ant target that does this job, add it to your build.xml file:

{lang="xml"}
~~~~~~~~
<!--
Check the file named in the property file.to.check (the clons.xml file) to see
if there are any clones.

The way this works is to find all lines containing the text "cloneClass" and put them into a separate file.  Then it checks to see if this file has non-zero length. If so, then there are errors, and it sets the property clones.found. Then it calls the fail-if-clones-found target, which doesn't execute if the clones.found property isn't set.
-->
<target name="check-clones-file"
        description="Checks the file (specified in ${file.to.check}) for clones">
       <property name="file.to.check" description="The file to hold the clones" />
       <copy file="${file.to.check}" tofile="${file.clonecount}">
              <filterchain>
                     <linecontains>
                           <contains value="cloneClass" />
                     </linecontains>
              </filterchain>
       </copy>

       <condition property="clones.found" value="true">
              <length file="${file.clonecount}" when="gt" length="0" />
       </condition>

       <antcall target="fail-if-clones-found" />
</target>

<!-- Fail the build if clones detected-->
<target name="fail-if-clones-found" if="clones.found">
       <echo message="Code clones found - setting build fail flag..." />
       <fail message="Code clones detected during ${codeline} build.  Check logs." />
</target>
~~~~~~~~

Note: In order to pass parameters to the ant script, click on the advanced button and add parameters as in the following screen:

![Passing parameters to Ant](\images\ant_params.png)
