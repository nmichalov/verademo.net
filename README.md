# Verademo.NET

## VSTS Inital Setup

Make sure you have the Veracode VSTS plugin installed from the marketplace, browse at the top right of the VSTS UI to find and install.

First set up a new project in VSTS. Use `git` as the version control system and whatever 'work item process' you prefer (default is fine). Check the 'code' tab for your new repository to set up 'git credentials' that you will need when pushing the code to VSTS.

Download [this project](https://gitlab.laputa.veracode.io/solutions-architecture/verademo.net/repository/master/archive.zip) to your local machine and unzip.

Initialise a Git repository in the unzipped project:

    git init
    git add *
    git commit -m "Inital commit"
  
Then push this project up to VSTS (this is where you will be asked for credentials as created earlier):

    git remote add origin https://<your-vsts-subdomain>.visualstudio.com/_git/<your-project-name>
    git push -u origin --all

Once complete, create a new build definition under the 'build and release' tab in VSTS. At the time of writing you probably want to select the 'ASP.NET (Preview)' template and customise as follows.

## Build Definition Customisation

MSBuild arguments for pre-compilation using a publish profile, to make the publish profile see the [help center](https://help.veracode.com/reader/4EKhlLSMHm5jC8P8j3XccQ/LlBZOwSnuA9xKGTopWDhZQ) or [this snippet](https://gitlab.laputa.veracode.io/solutions-architecture/verademo.net/snippets/44)

Pre-compiled files are output to `./publishfiles` relative to the root build directory as per the [`veracode.pubxml`](https://gitlab.laputa.veracode.io/solutions-architecture/verademo.net/blob/master/Verademo-Dotnet/Properties/PublishProfiles/veracode.pubxml) publish profile in this repo

![MSBuild Arguments](https://gitlab.laputa.veracode.io/solutions-architecture/verademo.net/raw/master/Docs/Images/build.png "MSBuild Arguments")


Recent versions of VSTS/TFS will make a `roslyn` folder than needs removing

![Delete Roslyn directory](https://gitlab.laputa.veracode.io/solutions-architecture/verademo.net/raw/master/Docs/Images/delete.png "Delete Roslyn directory")


Copy any required PDB files to the directory that is about to be zipped for upload, in this case `Verademo-dotnet.pdb`

![Copy files](https://gitlab.laputa.veracode.io/solutions-architecture/verademo.net/raw/master/Docs/Images/copy.png "Copy files")


Make the aforementioned zip file from the `./publishfiles/bin` directory

![Create zip file](https://gitlab.laputa.veracode.io/solutions-architecture/verademo.net/raw/master/Docs/Images/archive.png "Create zip file")


And finally upload and scan

![Upload and scan](https://gitlab.laputa.veracode.io/solutions-architecture/verademo.net/raw/master/Docs/Images/uploadandscan.png "Upload and scan")