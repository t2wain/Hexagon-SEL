# Hexagon ISL (Intergraph Smart Licensing)

## License Usage Information

Hexagon has a web service that provides information about the usages of the software licenses. The following information can be obtained from the web services:

- Real-time license usages
- Historical license usages
- Total quantity and type of software licenses

Real-time license usage include the following information:

- Active licenses being used and the remaning time to expire
- Users information include logon ID, computer name, date, time, and duration
- Active licenses avaliable for use and the remaning time to expire
- Expired licenses and the remaing times to become available

Historical license usage include the forllowing information:

- Date and time of license checkout and check-in by users
- User infomation include logon ID and computer name

To have a full picture of license usage, we saved a snapshot of the real-time usage information every 5 minutes. By comibining the real-time and historical usage information, we can develop reports and charts and made available for admins and users to monitor the availability of software licenses in real-time and plan for future purchases based on historical usages. 

One missing piece of information is the data on failed attemptS to obtain a license when all licenses are currently in use. Such information was available from the previous license servers (SPLM) but not from the ISL. This information can provide us with additional insight on the level of need for the licenses.  

## Accessing ISL Web Service

You can request Hexagon for access to the ISL web service. Hexagon will provide you with a Microsoft .NET code sample to get you started. Hexagon implemented the web service using the OData protocol which is an open standard technology. You can learn much more about OData from the web.

Hexagon will provide you with the following information needed to access the web service that are specific to your organization:

- Client ID
- Client Secret
- Company Code

The general steps are:

- Call the Identity web service to authenticate and generate a token which has a time-limited life span
- Save the token and use the token on subsequent calls to the ISL web service

Being an OData web service, you can use a Microsoft Visual Studio extension to generate a proxy service class from the web service metadata that will make it more convenience to call the web service in your code.