# auto-scaling-demo Overview

The Auto Scaling Demo provides a visual demonstration of the Auto-Scaling service for Cloud Foundry apps in IBM Cloud [also known as Bluemix]. It helps to demonstrate how policies defined in the Auto-Scaling service can horizontally scale any application based on CPU utilization and memory usage. To showcase these features,
the application contains visual gauges to indicate changing environmental performance metrics.


## Running the app on Bluemix

1. If you do not already have a Bluemix account, [sign up here][bluemix_signup_url]

2. Download and install the [Cloud Foundry CLI][cloud_foundry_url]

3. Clone the app to your local environment from your terminal using the following command

  ```
  git clone https://ghe.aa.com/EIS-PHS/auto-scaling-demo.git
  ```

4. cd into this newly created directory

5. Edit the manifest.yml file and change the <application-host> parameter to something unique. For Ops Training class, use your Student ID number.

  ```
  applications:
  - name: auto-scaling-demo-0001
    framework: node
    runtime: node12
    memory: 128M
    instances: 1
  ```
  The host you use will determinate your application url (e.g. `<application-name>.mybluemix.net`)

6. Connect to Bluemix using the CF CLI and follow the prompts to log in.

  ```
  $ cf api https://api.ng.bluemix.net
  $ cf login
  ```

7. Create the Auto-Scaling service in Bluemix. Remember to make this your Student ID number or something else unique. Duplicate services are possible but not a good idea unless you are collaborating with a team.

  ```
  $ cf create-service Auto-Scaling free Auto-Scaling-0001
  ```

8. Push your app to Bluemix

  ```
  $ cf push
  ```

9. Go to the Bluemix dashboard and enter the Auto-Scaling service console. Create an auto-scaling policy called 'MemoryPolicy' with the following parameters:
	* Metric type: `Memory`
	* If average CPU utilization exceeds `60%`, then increase `1` instance
	* If average CPU utilization is below `40%`, then decrease `1` instance
	* Statistic Window: `30 seconds`
	* Breach Duration: `60 seconds`
	* Cooldown period for scaling out: `600 seconds`
	* Cooldown period for scaling in: `600 seconds`

And voila! You now have your very own instance of Auto Scaling Demo running on Bluemix. To test it out, run a few load tests on your app using a service like [BlazeMeter][blazemeter_url] or jmeter from the CLI. If you want a more in depth tutorial on how to use and test auto scaling with this app, check out the [Handle the Unexpected with Bluemix Auto Scaling][auto_scaling_blog] blog post on developerWorks.


### Troubleshooting

To troubleshoot your Bluemix app the main useful source of information is the logs. To see them, run this (Remember to replace 0001 with your Student ID):

  ```
  $ cf logs auto-scaling-demo-0001 --recent
  ```
