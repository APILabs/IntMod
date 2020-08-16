# ACE - Using MQ policy with ACE

1. Create a new directory to be used as an integration server's work directory. 

    mqsicreateworkdir /home/mdu/myaceworkdir

1. Start the IntegrationServer
    
 ```   IntegrationServer -w /home/mdu/myaceworkdir ```


1. Run the mqsisetdbparms command to set MQ security credentials for connection to MQ.

```    mqsisetdbparms -w /home/mdu/myaceworkdir -n mq::mqopenshift -u app -p Passw0rd ```

1. Create MQ policy.

Procedure

    4.1 In the Application Development view of the IBM App Connect Enterprise Toolkit, click New, then select Policy.
    4.2 Select an existing policy project or click New to create a policy project.
    4.3 Enter a name for your policy. The policy name can include alphanumeric characters and the underscore character. The name cannot include spaces and must start with a letter.
    4.4 Click Finish, A .policyxml file is created and is opened in the Policy editor.
    4.5 In the Policy editor, select the policy type from the list.
    4.6 If your chosen policy type has more than one template, select the appropriate template from the list. The template provides default values for some fields.
    4.7 Edit or set appropriate values for your policy. Mandatory properties are marked with an asterisk. For more information about property values for specific policies, see Policy properties.
    4.8 Save your policy. Any errors are reported on the Problems tab.

Sample .policy file :
**MQ_Policy.policyxm**

```
 <?xml version="1.0" encoding="UTF-8"?>
 <policies>
   <policy policyType="MQEndpoint" policyName="MQ_Policy" policyTemplate="MQEndpoint">
     <connection>CLIENT</connection>
     <destinationQueueManagerName>QM1</destinationQueueManagerName>
     <queueManagerHostname>icp-proxy.40.118.70.236.nip.io</queueManagerHostname>
     <listenerPortNumber>32744</listenerPortNumber>
     <channelName>EXAMP.ADMIN.SVRCONN</channelName>
     <securityIdentity>mqopenshift</securityIdentity>
     <useSSL>false</useSSL>
     <SSLPeerName></SSLPeerName>
     <SSLCipherSpec></SSLCipherSpec>
   </policy>
 </policies>
``` 


1. What to do next

 1. Attach the policy to a message flow node by setting the policy name in the appropriate property of the message flow node.
 1. If the policy is deployed in the default policy project for the integration server, specify just the policy name in the message flow property.If the policy is deployed in a non-default policy project, prefix     the name of the policy with the policy project name:    
    ```    {PolicyProjectName}:PolicyName, i.e. {MyPolicies}:MQ_Policy ```
 1. Deploy the policy, you can deploy one or more policy projects, either in an independent BAR file, or in the same BAR file as the associated message flow. 