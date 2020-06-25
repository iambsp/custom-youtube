# Custom Youtube (Alexa Project)
The purpose of this project is to enable users to listen to YouTube-sourced audio on their Alexa devices. This skill searches for a set of YouTube videos based on a search term provided by the user, and then plays the audio from the most relevant video, while enqueueing the next most-relevant tracks to be played after. There are also several options provided to manage playback settings, namely: previous, next, pause, resume, and repeat mode (replay the current song indefinitely).

For usage instructions, scroll down towards the bottom of this README, past the Setup instructions.

\* [https://github.com/fent/node-ytdl-core](https://github.com/fent/node-ytdl-core) is used to access the audio from YouTube. Additionally, the Amazon Audio Play Sample Project (specifically the [multiple-streams skill]([https://github.com/alexa/skill-sample-nodejs-audio-player/tree/mainline/multiple-streams](https://github.com/alexa/skill-sample-nodejs-audio-player/tree/mainline/multiple-streams))) was consulted for best practice information in using the Alexa Audio Player Interface

## Manual Setup/Installation
### Prerequisites:
1. You will need an [Amazon developer account](https://developer.amazon.com/) to create an Alexa Skill. It is important that you use **the same email** to setup this account that you have the Alexa devices registered to. In doing so, you will enable custom development Skills to be easily called from your devices.
2. You will also need an [AWS account](https://aws.amazon.com/) in order to upload the lambda code that will handle your Alexa devices' requests.
3. You will also need a YouTube API Key. In order to get this, follow steps 1-3 [here]([https://developers.google.com/youtube/v3/getting-started](https://developers.google.com/youtube/v3/getting-started)). After doing so you should be able to access and create additional API keys on the [Credentials page](https://console.developers.google.com/apis/credentials). It is important to note that you only need one key, and do not need OAuth authorization.
4. (Optional): If you intend to build the project from source (instead of using the zip file which includes the node modules and is located in the dist folder), you will also need [NodeJS]([https://nodejs.org/en/download/](https://nodejs.org/en/download/)) and npm (Node Package Manager) installed. If you simply want the skill to work, feel free to disregard this requirement.

After these accounts are created (they are both free, but make sure you understand AWS free tier limits), you will be ready to move on.

\* If there is enough demand, I will add ask-cli installation compatibility and instructions
### Initial Alexa Skill Setup:
1. Once logged into your Amazon developer account, from the [Alexa console]([https://developer.amazon.com/alexa/console/ask](https://developer.amazon.com/alexa/console/ask)) click the 'Create Skill' button.
2. On this page, enter the Skill Name (I used CustomYoutube, but feel free to pick your own) and use English(US) as the default language. Choose 'Custom' for your model, and 'Provision your own' for the backend resource method. Click 'Create skill' to confirm. ![Alexa Create Skill](https://github.com/cosmosis14/custom-youtube/blob/master/assets/alexaCreateSkill.png?raw=true)
3. On the next page, choose the 'Hello World Skill' template, and confirm by pressing the 'Choose' button.  This will now take  a couple of seconds to create your Skill, before sending you to the new Skill's homepage.
4.  On the Skill's homepage, click on 'Interfaces' in the sidebar, and then enable the 'Audio Player interface (after enabling, it should look like the picture). To finish this step, press the button labeled 'Save Interfaces'. ![Alexa Interfaces](https://github.com/cosmosis14/custom-youtube/blob/master/assets/alexaInterfaces.png?raw=true)
5. Next, either download the file located at https://github.com/cosmosis14/custom-youtube/blob/master/dist/en-US.json on to your computer, or make sure you copy all of this file's contents into your clipboard (Ctrl+C). If you are unfamiliar with GitHub, you can download the file by clicking on the 'Raw' button near the top of the page, and then on the following page, right-clicking anywhere and choosing 'Save as...'
6. Click 'Interaction Model' in the sidebar, which should reveal a drop-down menu. On that new menu, click on 'JSON Editor'. In the JSON Editor page, either click on the 'Drag and drop a .json file' area and upload the file you downloaded in the previous step, or if you simply copied the text from the file, paste it into the textbox (labeled 3* in the picture). Now click 'Save Model' at the top. ![Alexa Interaction Model](https://github.com/cosmosis14/custom-youtube/blob/master/assets/alexaInteractionModel.png?raw=true)
7.  Click 'Endpoint' in the sidebar, and make sure that the AWS Lambda ARN radio button is selected. Next, click the 'Copy to Clipboard' button under the 'Your Skill ID' section. Now, open a new tab, but leave this page open, as we will be returning to it shortly. ![Alexa Endpoint](https://github.com/cosmosis14/custom-youtube/blob/master/assets/alexaEndpoint.png?raw=true)

### AWS Lambda Setup:
1. On the new open tab, login to your AWS account, and navigate to [the Lambda Functions page](https://console.aws.amazon.com/lambda/home). Then, in the top-right corner (labeled 1 in the picture), choose a region for the code we will be uploading. Choosing a location geographically close to you should speed up the performance of the Skill, however, some regions are limited, so if you experience problems you may want to research this topic, or just be safe and choose us-east-1 (N. Virginia). Next, click the 'Create function' button. ![AWS Create Function](https://github.com/cosmosis14/custom-youtube/blob/master/assets/awsCreateFunction.png?raw=true)
2. In the next page, choose the 'Author from scratch' option, name your Function (I chose CustomYoutube, but you can name it what you like), choose Node.js 12.x for the runtime, and after clicking the 'Choose or create an execution role' dropdown, make sure that 'Create a new role with basic Lambda permissions' is selected. Lastly, click 'Create function'. ![AWS Create Function 2](https://github.com/cosmosis14/custom-youtube/blob/master/assets/awsCreateFunction2.png?raw=true)
3. Once your function has been created, and you are on the configuration page, in the 'Designer' area, click the 'Add trigger' button on the left. On this next page, select 'Alexa Skills Kit' from the dropdown menu. Make sure that the 'Skill ID verification' option is set to 'Enable (recommended)'. Next, paste the Skill ID that you copied from Step 7 of the Initial Alexa Skill Setup into the textbox labeled 'Skill ID'. Then click the 'Add' button ![AWS Designer](https://github.com/cosmosis14/custom-youtube/blob/master/assets/awsDesigner.png?raw=true)![AWS Add Trigger](https://github.com/cosmosis14/custom-youtube/blob/master/assets/awsAddTrigger.png?raw=true)
4. For the next step, you will need to download the skill.zip file from this project, located at https://github.com/cosmosis14/custom-youtube/blob/master/dist/skill.zip. Once this file is on your computer, return to the AWS page.
5. After adding the Alexa Skills Kit trigger, AWS should have returned you to the main page for your function. If it is not already highlighted, click on the area that says CustomYoutube (labeled 1 in the picture) in the 'Designer' area. This will reveal the 'Function code' area beneath it.  In the 'Function code' area, click the 'Actions' button and select 'Upload a .zip file'. In the new dialog popup, click the upload button, and choose the .zip file you downloaded in the previous step. Then click the 'Save' button (still in the popup dialog). ![AWS Function upload](https://github.com/cosmosis14/custom-youtube/blob/master/assets/awsFunctionUpload.png?raw=true)
6. Now, once your code has been uploaded and saved, scroll down on the function page, below the 'Function Code' area and a couple other sections, until you get to the 'Basic settings' section. Click the 'Edit' button on this section. On the following page, update the 'Timeout' option from '0 min 3 sec' to '0 min 30 sec'. Click the 'Save' button at the bottom of the page. ![AWS Basic Settings](https://github.com/cosmosis14/custom-youtube/blob/master/assets/awsBasicSettings.png?raw=true)
7.  Once more this will bring us back to the main function page. Go to the 'Function code' section, and click into the text area showing our uploaded index.js file. Press Ctrl+f to enter search mode, and type in 'api key' into the search textbox. This will highlight a line of code (line 756 at the time of this writing) that says:
// keyParam: 'UNCOMMENT AND INSERT YOUR YOUTUBE DATA API KEY HERE'
Follow the advice of this message, and delete the two forward slashes at the start of the line (this uncomments the line), and then delete all of the capitalized text and replace it with your YouTube Data API Key (check Step 3 of the Prerequisites for instruction to get this key). Make sure not to delete the single quotes that will surround your key. When finished with this step, save the code change by pressing the 'Save' button in the top right of the page. ![AWS API Key 1](https://github.com/cosmosis14/custom-youtube/blob/master/assets/awsApiKey1.png?raw=true)![AWS API Key 2](https://github.com/cosmosis14/custom-youtube/blob/master/assets/awsApiKey2.png?raw=true)
8. Before we leave this page, scroll up to the top of the page, and copy the ARN for your function (either to your clipboard, or some place more permanent), as we will be using that shortly. Then, click on the 'Permissions' tab near the top of the page. Now, you should see an area of the page titled 'Execution role', in this area click on the link below 'Role name'. The link should be prepended with your function name (in my case, CustomYoutube). ![AWS ARN](https://github.com/cosmosis14/custom-youtube/blob/master/assets/awsARN.png?raw=true)
9. This should bring you to a page describing the permissions of your Lambda function. Here, click the button labeled 'Attach policies'. You will now be on a page with a search bar near the top. Enter 'AmazonDynamoDBFullAccess' into this search bar, and click the checkbox to the left of the policy with this same name. Then click the 'Attach policy' button in the lower-right corner. ![IAM Permissions](https://github.com/cosmosis14/custom-youtube/blob/master/assets/iamPermissions.png?raw=true)![IAM Permissions Add](https://github.com/cosmosis14/custom-youtube/blob/master/assets/iamPermissionAdd.png?raw=true)

### Connecting AWS Lambda Function to Alexa Skill: 
Oofda! We are almost done, what a journey!

1. Now, go back to your tab with the Alexa Skill Endpoint page open, this was first referenced in Step 7 of the Initial Alexa Skill Setup (I told you we would be back shortly, but shortly might have been an exaggeration...). Here, under your Skill ID there is a textbox labeled 'Default Region'. Paste the ARN we copied at the beginning of Step 8 of the AWS Lambda Setup into this textbox, and then click the 'Save Endpoints' button near the top of the page. Then, after saving, click the 'Invocation' tab in the sidebar. ![Alexa Endpoint Arn](https://github.com/cosmosis14/custom-youtube/blob/master/assets/alexaEndpointARN.png?raw=true)
2. On the 'Invocation' page, you can change the Skill Invocation Name if you desire. This name is the term that users will refer to the skill as when invoking it with an Alexa device. So if you were to update the name from 'custom youtube' to "don's youtube" for example, then to invoke the skill, you would say "Alexa, launch don's youtube" when addressing an Alexa device. After choosing your 'Invocation Name' (or leaving it as 'custom youtube'), click the 'Save Model' button at the top of the page, and then the 'Build Model' button to the right of that. Your Skill will now take about a minute to build. when it is done, it will notify you (do not worry about the 'Utterance Conflicts Detected' warning). Then, click into the header in the top menu bar of the page labeled 'Test'. ![Alexa Invocation](https://github.com/cosmosis14/custom-youtube/blob/master/assets/alexaInvocation.png?raw=true)
3. On this page, you will see a message telling you 'Test is disabled for this skill.' and a dropdown menu next to this message. In the dropdown option, change the selection from 'Off' to 'Development'. And that's it, we're done! On this page, you can either run a simulated Alexa test by typing commands to Alexa, in the text box labeled 2\* in the picture, or (if you enabled microphone use on the page) by clicking and holding the mic icon in the same box and speaking the command (remember to use the invocation name you set). Furthermore, testing on all local Alexa devices (those registered to the email that you set up the Amazon Developer account under) is also enabled, meaning the skill is fully accessible on your devices. ![Alexa Test](https://github.com/cosmosis14/custom-youtube/blob/master/assets/alexaTest.png?raw=true)

*Note that on the first invocation of the Skill, the database needs to set up, which takes about 10 seconds. The Skill will tell you that this setup is occurring, and the next time you invoke it, it should operate correctly.

## Usage

As noted at the end of the previous section, your first invocation of the skill, started by telling an Alexa device to "Launch Custom Youtube" will result in a response informing you that the database is being configured. After waiting about 10 seconds, the skill should be ready to use.

### Commands:
\* All commands must be prepended with the device 'Wake Word' (typically 'Alexa'). Also, if a part of a command below is surrounded by curly brackets, i.e. "{ }", this is meant to be representative of potential user input, and you are meant to substitute the word(s) in the curly brackets with an appropriate term.

1. General Commands
* "Launch Custom Youtube" or simply "Custom Youtube"
	* Starts the skill, which will prompt you for the name of a song (or other audio) that you would like to hear
	* If playback had been stopped prior to the launch, the skill will ask you if you would like to resume playback
* "Ask Custom Youtube to play {Song Name}"
	* Loads the requested audio from YouTube and begins playback
* "Ask Custom Youtube for help"
	* Returns a short help message with some possible commands to use

2. Commands to use during playback
* "Play", or "Resume", "Stop", "Previous", "Next", and "Start Over"
	* These commands are mostly self-explanatory.  "Previous" and "Next" will change the currently playing song to either the one playing before it (if applicable), or the next song in the queue. There are 20 songs by default in each queue, and after the 20th song, playback will loop back to the 1st song. "Start Over" will replay the current song from the beginning.
* "Ask Custom Youtube to repeat"
	* Turns on Single Repeat Mode. This will result in the currently playing song to repeat indefinitely (until either Repeat Mode is turned off (see following command), a Stop/Previous/Next/Start Over command is given, or a new song is requested to play).
* "Ask Custom Youtube to stop repeating" of "Ask Custom Youtube to turn repeat off"
	* Turns off Single Repeat Mode
* "Ask Custom Youtube what song this is" or "Ask Custom Youtube for song info"
	* Returns the title of the YouTube video that the currently playing audio was sourced from