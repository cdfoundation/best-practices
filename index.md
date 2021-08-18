# Continuous Delivery Foundation Best Practices


## Version Control 
(To be added)


## Continuous Integration (CI)
### Why CI
[Continuous Integration](https://github.com/cdfoundation/glossary/blob/main/definitions.md#continuous-integration) ensures that coding changes are merged, compiled/linked, packaged and registered on a frequent basis, avoiding broken builds and ensuring a clean release candidate is available continuously. 

### Definition
Continuous Integration, the CI in CI/CD, is the practice of combining code changes frequently, where each change is verified on check-in.

Examples of verifications:
* Code scanning
* Testing
* Building and packaging


### Description & Scope
Minimizing broken builds due to incompatible coding changes is the purpose of the continuous integration process. Many of us can remember the days when project teams would have a ‘sync-up’ process which generally meant check-in all of your coding updates and let us all ‘pray’ the build runs. An unsung hero called the Build Manager created the build script which would include merging and pulling all of the coding updates based on ‘tags’ and then get the build to run. 

This ‘sync-up’ step was performed once a week, every two weeks or monthly. It was not unusual for the build manager to put in a solid 8-12 hours to get the build working. The resulting build was often referred to as a ‘clean’ build which created an available release candidate. This process meant you would only have a release candidate to pass to testing on a very low-frequency basis, which in turn slowed down the entire application lifecycle process. Builds were always the bottleneck. 

Continuous integration changed the way the build (merge, compile/link, package and register) step was implemented. By triggering the process on a check-in of code to the versioning system, continuous integration quickly identified if a developer broke the build when they introduced new code. In essence, the process of merging, compiling and linking code on a high-frequency basis allows for the continuous integration of coding changes as soon as possible. If the build breaks due to a coding update, the build is easier to fix with smaller incremental changes, versus the ‘sync-up’ method where dozens of possible coding changes impacted the build leaving it to the build manager to sort out all of the problems - a tedious and onerous process. And more frequent builds meant that testing got more frequent updates, truly the beginning of ‘agile’ or incremental software updates. 

The process of triggering the ‘build’ is sometimes referred to as the continuous build step of the CI process. This step is executed by calling a static build script created and maintained by the build manager. It is in this step that all of the software configuration management is performed which includes determining what code to pull, what libraries to use and what parameters must be passed to the compilers and linkers.  The ‘build’ step of CI is triggered by a source code ‘check-in’ event.  It then executes a workflow process to run the build script and create, package and register the new binaries and thereby create a new release candidate. 

As CI matured, so did the process around the central theme. CI workflows were created by developers to not only run the build script, but also deploy the new binaries to a development endpoint followed by running automated testing. In addition, code and library management steps were added to the build improving the quality and security of code, and ensuring the usage of the correct transitive dependencies, open source licenses, and security scans were done during the creation of the release candidate.   

### Best practices

#### Merging
Defining a merge strategy is best discussed in the version control section of this document. However, as merging relates to the CI build process, there are some basic best practices to consider. Keep in mind, merging is triggered after a pull request has been approved. 

##### Know your Merging Strategy
In your build step, it is important to understand what code is being pulled from version control to be compiled. For this reason, there should be clear best practices defined for managing branches. 

* **Compile by Branch** - for every build, a branch is referenced. The branch name is passed to the build step to determine what to pull for the compile/link. 

* **Compile by Tag** - a Tag has been applied to all objects in the repository and the build pulls the code based on the Tag for the compile step. The Tag is a collection of objects that relate together. 

#### Compile/Link Best Practices
The creation of build scripts and how they are managed can often be controversial. Writing a build script is not easy in large monolithic practices. Whether it be a build script for Java or more complex C++ code, it is a tedious and time consuming process. There are some basic guidelines that should be followed for creating the build scripts that the CI process will run.

##### Build Work Products
Regardless of what type of build is executing, it should produce 3 basic outputs.
1. A build should create not only the binaries, but also the application package named based on a version numbering schema that relates back to the versioning Tag. (MSI, zip, rpm, or container image)
2. A full Bill of Material report should be required at minimum for all production releases. BOM reports are often undervalued, but they are key to debugging issues if needed. A BOM report should show:
    * All source code included in the build.
    * All libraries or packages, internal and external used in the link.
    * All compile/link parameters used to define the binaires.
    * Licensing of external components and transitive dependencies. 
3. Every build should include a Difference report. A Difference report shows what changed between any two builds. This should be used for approving updates before a release to testing or production environments. A Difference report should be generated based on a comparison of two BOM reports. Difference reports can be pulled from the version repository, but may be incomplete as objects such as third-party libraries are not pulled from a version repository. 

##### Do not use wildcard includes (/*.*)
When defining where code, packages and libraries are to be found in the build process, do not use wildcard includes. Instead, list all file references by name that need to be compiled or linked into the binary objects. While this may seem a lot of extra work, it is essential in securing that only approved objects end up in the resulting binary. If you use the wildcard includes, non-approved objects will no doubt be delivered with your binary. This can be a hidden security risk. This also means your binary includes unnecessary objects which can substantially increase the size of your binary. 

##### Know Your Build Parameters
Build parameters determine how the resulting binaries are created. For example, the use of debug flags allows the binaries to include debug symbols that can be exploited and should not be deployed to production environments. 

##### Binary Repository
The CI build should include the updating of the binaries to an appropriate binary repository. The binary repository should be used to persist and share the binaries to the continuous deployment step of the Continuous Delivery Pipeline. These binaries should be tagged with the corresponding version control ‘tag.’  

#### Docker Specific Best Practices
A Multi-stage Docker Build is the process of moving your CI steps into a container build. It is called ‘multi-stage’ because it aggregates all of the needed steps into a Docker Build of the image. For example, in stage one run the compile/link steps to create the artifacts, stage two copy the artifacts to a run-time and destroy stage one. The benefit of considering a Multi-stage Docker Build is to create an airtight environment in which the process is executed. The container holds all objects without the possibility of an external change. Interesting to the Multi-stage process is the ability to move your entire CI Build process into a single container build. Best practices related to each of those steps should still be followed. This option minimizes external updates to the build step making it a best practice candidate. 



# To be Deleted

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/cdfoundation/best-practices/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
