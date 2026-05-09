🛡️ Detecting Malicious Files Using VirusTotal Integration with Wazuh
=====================================================================

📌 Overview
-----------

This lab demonstrates how to integrate Wazuh with VirusTotal to automatically detect malicious files using hash-based reputation analysis.

Using File Integrity Monitoring (FIM), Wazuh monitors file activity on an endpoint and queries VirusTotal whenever suspicious files are added or modified. If the file hash is identified as malicious, Wazuh immediately generates a security alert.

🎯 Objectives
=============

✔ Integrate Wazuh with VirusTotal✔ Enable real-time File Integrity Monitoring (FIM)✔ Detect malicious files using reputation-based scanning✔ Generate automated alerts for suspicious files✔ Validate detection using the EICAR anti-malware test file

🧪 Lab Environment
==================

ComponentDescription🖥️ SIEM/XDRWazuh🐧 Endpoint OSUbuntu🔍 Threat IntelligenceVirusTotal📂 Monitoring MethodFile Integrity Monitoring (FIM)☣️ Test MalwareEICAR Test File

⚙️ Prerequisites
================

Before starting, ensure the following components are ready:

*   ✅ Wazuh Manager installed and running
    
*   ✅ Wazuh Agent connected to endpoint
    
*   ✅ VirusTotal API key generated
    
*   ✅ File Integrity Monitoring enabled
    
*   ✅ Internet access available
    

🧩 Step 1 — Enable File Integrity Monitoring (FIM)
==================================================

🔗 Connect to Ubuntu Endpoint
-----------------------------

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   ssh wazuh-user@192.168.209   `

Switch to root:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sudo su   `

Open the Wazuh agent configuration:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   mousepad /var/ossec/etc/ossec.conf   `

📝 Configure Syscheck
---------------------

Locate the section and add:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML  `no  /root`

📖 Configuration Breakdown
--------------------------

SettingPurposecheck\_all="yes"Monitor all file attributesreport\_changes="yes"Detect content modificationsrealtime="yes"Enable real-time monitoring

📌 Wazuh will now continuously monitor the /root directory for changes.

🧠 Step 2 — Create Custom Detection Rules
=========================================

🛠️ Navigate to Local Rules
---------------------------

Open:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   Wazuh Dashboard   └── Rules       └── Manage Rules Files           └── local_rules.xml   `

Add the following rules:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML      `550    /root    File modified in /root    554    /root    File added to /root`  

🚨 Rule Functionality
---------------------

Rule IDDescription100200Detects modified files100201Detects newly created files

These rules generate alerts whenever file activity occurs inside /root.

🌐 Step 3 — Integrate VirusTotal
================================

🔑 Generate VirusTotal API Key
------------------------------

Visit:

[VirusTotal Official Website](https://www.virustotal.com?utm_source=chatgpt.com)

Generate and copy your API key.

⚙️ Configure Integration
------------------------

Open the Wazuh manager configuration:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sudo mousepad /var/ossec/etc/ossec.conf   `

Add the following integration block:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML  `virustotal  YOUR_API_KEY  100200,100201  json`

📖 Integration Details
----------------------

ParameterDescriptionnameEnables VirusTotal integrationapi\_keyVirusTotal authentication keyrule\_idRules that trigger hash lookupalert\_formatSends alerts in JSON format

☣️ Step 4 — Download EICAR Test File
====================================

The EICAR file is a safe anti-malware testing file used to simulate malware detection.

Switch to root:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sudo su   `

Download the file:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sudo curl -o /root/eicar.com https://secure.eicar.org/eicar.com && sudo ls -lah /root/eicar.com   `

🔍 Detection Flow
=================

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   EICAR File Downloaded          │          ▼Wazuh FIM Detects File Creation          │          ▼Custom Rule Triggered (100201)          │          ▼VirusTotal Hash Lookup          │          ▼Malicious Reputation Found          │          ▼Security Alert Generated   `

📊 Expected Alert in Wazuh
==========================

FieldExpected Value📄 File Nameeicar.com🆔 Rule ID100201🔍 IntegrationVirusTotal☣️ DetectionMalicious File🚨 Alert LevelHigh

🖼️ Sample Workflow Architecture
================================

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   ┌─────────────┐│ Ubuntu Host │└──────┬──────┘       │       ▼┌────────────────────┐│ Wazuh Agent (FIM)  │└──────┬─────────────┘       │ File Event       ▼┌────────────────────┐│  Wazuh Manager     │└──────┬─────────────┘       │       ▼┌────────────────────┐│ VirusTotal Lookup  │└──────┬─────────────┘       │       ▼┌────────────────────┐│ Security Alert     │└────────────────────┘   `

🔐 Security Benefits
====================

✅ Real-time malware detection✅ Automated threat intelligence checks✅ Faster incident response✅ Improved endpoint visibility✅ Reduced manual analysis effort✅ Enhanced SOC monitoring capability

🏁 Conclusion
=============

This lab successfully demonstrated the integration of Wazuh with VirusTotal for automated malicious file detection.

By combining:

*   📂 File Integrity Monitoring (FIM)
    
*   🌐 VirusTotal Threat Intelligence
    
*   🚨 Custom Detection Rules
    

Wazuh can automatically identify suspicious file activity and determine whether a file is malicious based on global reputation data.

The EICAR test confirmed that the detection pipeline works correctly and generates real-time security alerts for potentially malicious files.
