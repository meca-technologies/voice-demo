#Objective metrics used to measure the quality of audio, such as the Signal-to-Noise Ratio (SNR), the Mean Opinion Score (MOS), and the Perceptual Evaluation of Speech Quality (PESQ).

Location based community app
Build Using react Native, We're trying to bring people together by allowing them to find courts (basket ball, soccer, football) nearby.

![home-12](https://user-images.githubusercontent.com/15659840/187789415-99f18d8b-28cf-461d-8314-3701f55036ab.png)


Installation. both IOS and Android
In order to setup for React Native template you should connect to the internet and install dependencies library.

this is important, please make sure you have already setup your environment. Node & NPM installation this package globally on your machine.

Install node and npm: Download it from here --> https://docs.npmjs.com/getting-started

	 
          node --version
          npm --version
          
 And run the simulator mobile iOs or Android base on this guide https://facebook.github.io/react-native/docs/getting-started.html, it will help to started with installing your first application with React Native apps.        

Next, Download the repo.

In the folder "My Mobile Application" you have one zip file.
# If you're running linux, unzip the folder in CLI

Extract the file project source code and add it to your workspace.

Open the folder project in the terminal / command line,

cd /MyMobileApplication/MyTurfLocator
run the command and run the following command:

 
	npm insatll
	or
	yarn install 
		
This will install all the required modules for your app.


Since this app will run on both android and IOS, i've adding this example to identify which platform it's running on.

       import { Platform, StyleSheet } from 'react-native';
       const styles = StyleSheet.create({
       height: Platform.OS === 'ios' ? 200 : 100
       });
