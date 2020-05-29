## Generate Jmeter Scripts By Code

```java
import com.blazemeter.utility.CommonUtils;
import org.apache.jmeter.config.Arguments;
import org.apache.jmeter.config.CSVDataSet;
import org.apache.jmeter.config.gui.ArgumentsPanel;
import org.apache.jmeter.control.IfController;
import org.apache.jmeter.control.LoopController;
import org.apache.jmeter.control.gui.IfControllerPanel;
import org.apache.jmeter.control.gui.LoopControlPanel;
import org.apache.jmeter.control.gui.TestPlanGui;
import org.apache.jmeter.engine.StandardJMeterEngine;
import org.apache.jmeter.extractor.json.jsonpath.JSONPostProcessor;
import org.apache.jmeter.extractor.json.jsonpath.gui.JSONPostProcessorGui;
import org.apache.jmeter.gui.tree.JMeterTreeListener;
import org.apache.jmeter.processor.PostProcessor;
import org.apache.jmeter.protocol.http.control.Header;
import org.apache.jmeter.protocol.http.control.HeaderManager;
import org.apache.jmeter.protocol.http.control.gui.HttpTestSampleGui;
import org.apache.jmeter.protocol.http.gui.HeaderPanel;
import org.apache.jmeter.protocol.http.sampler.HTTPSampler;
import org.apache.jmeter.protocol.http.sampler.HTTPSamplerProxy;
import org.apache.jmeter.reporters.ResultCollector;
import org.apache.jmeter.reporters.ResultCollectorHelper;
import org.apache.jmeter.reporters.Summariser;
import org.apache.jmeter.save.SaveService;
import org.apache.jmeter.testbeans.gui.TestBeanGUI;
import org.apache.jmeter.testelement.TestElement;
import org.apache.jmeter.testelement.TestPlan;
import org.apache.jmeter.testelement.property.BooleanProperty;
import org.apache.jmeter.testelement.property.TestElementProperty;
import org.apache.jmeter.threads.ThreadGroup;
import org.apache.jmeter.threads.gui.ThreadGroupGui;
import org.apache.jmeter.util.JMeterUtils;
import org.apache.jmeter.visualizers.ViewResultsFullVisualizer;
import org.apache.jorphan.collections.HashTree;
import org.apache.jorphan.collections.ListedHashTree;

import java.io.File;
import java.io.FileOutputStream;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Set;


public class JMeterFromScratchCustom {

    public static void main(String[] argv) throws Exception {

        File jmeterHome = new File("D:\\apache-jmeter-5.2.1");
        String slash = "\\";

        if (jmeterHome.exists()) {
            File jmeterProperties = new File(jmeterHome.getPath() + slash + "bin" + slash + "jmeter.properties");
            if (jmeterProperties.exists()) {
                //JMeter Engine
                StandardJMeterEngine jmeter = new StandardJMeterEngine();

                //JMeter initialization (properties, log levels, locale, etc)
                JMeterUtils.setJMeterHome(jmeterHome.getPath());
                JMeterUtils.loadJMeterProperties(jmeterProperties.getPath());
                JMeterUtils.initLogging();// you can comment this line out to see extra log messages of i.e. DEBUG level
                JMeterUtils.initLocale();

                // JMeter Test Plan, basically JOrphan HashTree
                ListedHashTree testPlanTree = new ListedHashTree();

                //Introducing CSV Data Set
                CSVDataSet csvDataSet=getCSVDataSet(
                        "CSV Data Set Config",
                        ",",
                        "",
                        "D:/jmeter-from-code/TestData/TestSpecificData/test.csv",
                        true,
                        false,
                        true,
                        "shareMode.group",
                        "username,password,orgId",
                        true
                );

                // First HTTP Sampler - open example.com
                HashMap<String,Object> hashMap=new HashMap<String, Object>();
                hashMap.put("Content-Type","application/json");
                hashMap.put("loginId","${username}");
                hashMap.put("password","${password}");
                hashMap.put("orgId","${orgId}");

                HeaderManager headerManager=getHeaderManagerObject("Login_Header",hashMap);

                JSONPostProcessor jsonPostProcessor=getJSONPostProcessor(
                        "token_Extractor",
                        "token",
                        "token");


                HashTree sampler1=(HashTree) generateSampler(
                        "userLogin",
                        "https",
                        "abc.com",
                        "//session/login",
                        "POST",
                        true,
                        true,
                        "{}",
                        headerManager,
                        jsonPostProcessor);

                hashMap=new HashMap<String, Object>();
                hashMap.put("Content-Type","application/json");
                hashMap.put("loginId","${username}");
                hashMap.put("token","${token}");
                hashMap.put("orgId","${orgId}");

                headerManager=getHeaderManagerObject("getWorkOrderType_Header",hashMap);
                HashTree sampler2=(HashTree) generateSampler(
                        "getWorkOrderTypeDetails",
                        "https",
                        "abc.com",
                        "//query/read",
                        "POST",
                        true,
                        true,
                        CommonUtils.geTestCaseTestData("getWorkOrderTypeDetails.json").getAsJsonObject().toString(),
                        headerManager,
                        null);


               /* // Third HTTP Sampler - open blazemeter.com
                HTTPSamplerProxy blazemetercomSamplerOther = new HTTPSamplerProxy();
                blazemetercomSamplerOther.setDomain("blazemeter.com");
                blazemetercomSamplerOther.setPort(80);
                blazemetercomSamplerOther.setPath("/");
                blazemetercomSamplerOther.setMethod("GET");
                blazemetercomSamplerOther.setName("Open blazemeter other");
                blazemetercomSamplerOther.setProperty(TestElement.TEST_CLASS, HTTPSamplerProxy.class.getName());
                blazemetercomSamplerOther.setProperty(TestElement.GUI_CLASS, HttpTestSampleGui.class.getName());

                // Third HTTP Sampler - open blazemeter.com
                HTTPSamplerProxy blazemetercomSamplerOther1 = new HTTPSamplerProxy();
                blazemetercomSamplerOther1.setDomain("blazemeter.com");
                blazemetercomSamplerOther1.setPort(80);
                blazemetercomSamplerOther1.setPath("/");
                blazemetercomSamplerOther1.setMethod("GET");
                blazemetercomSamplerOther1.setName("Open blazemeter other");
                blazemetercomSamplerOther1.setProperty(TestElement.TEST_CLASS, HTTPSamplerProxy.class.getName());
                blazemetercomSamplerOther1.setProperty(TestElement.GUI_CLASS, HttpTestSampleGui.class.getName());*/


                // Loop Controller
                LoopController loopController = new LoopController();
                loopController.setLoops(2);
                loopController.setFirst(true);
                loopController.setProperty(TestElement.TEST_CLASS, LoopController.class.getName());
                loopController.setProperty(TestElement.GUI_CLASS, LoopControlPanel.class.getName());
                loopController.initialize();

                // Thread Group
                ThreadGroup threadGroup = new ThreadGroup();
                threadGroup.setName("BasicThreadGroup");
                threadGroup.setNumThreads(1);
                threadGroup.setRampUp(1);
                threadGroup.setSamplerController(loopController);
                threadGroup.setProperty(TestElement.TEST_CLASS, ThreadGroup.class.getName());
                threadGroup.setProperty(TestElement.GUI_CLASS, ThreadGroupGui.class.getName());

                // Test Plan
                TestPlan testPlan = new TestPlan("TestPlan for ABC");
                testPlan.setProperty(TestElement.TEST_CLASS, TestPlan.class.getName());
                testPlan.setProperty(TestElement.GUI_CLASS, TestPlanGui.class.getName());
                testPlan.setUserDefinedVariables((Arguments) new ArgumentsPanel().createTestElement());

                // Construct Test Plan from previously initialized elements
                testPlanTree.add(testPlan);
                ListedHashTree threadGroupHashTree = (ListedHashTree) testPlanTree.add(testPlan, threadGroup);

                threadGroupHashTree.add(csvDataSet);
                threadGroupHashTree.add(sampler1);
                threadGroupHashTree.add(sampler2);
                //threadGroupHashTree.add(blazemetercomSamplerOther);
                //threadGroupHashTree.add(blazemetercomSamplerOther1);
                //threadGroupHashTree.add(hashTreeOther);
                ResultCollector resultCollector=new ResultCollector();
                resultCollector.setProperty(TestElement.GUI_CLASS, ViewResultsFullVisualizer.class.getName());
                resultCollector.setProperty(TestElement.TEST_CLASS,ResultCollector.class.getName());
                resultCollector.setProperty(TestElement.NAME,"View Results Tree");
                resultCollector.setProperty(TestElement.ENABLED,"true");


                resultCollector=new ResultCollector();
                resultCollector.setProperty(TestElement.GUI_CLASS, ViewResultsFullVisualizer.class.getName());
                resultCollector.setProperty(TestElement.TEST_CLASS,ResultCollector.class.getName());
                resultCollector.setProperty(TestElement.NAME,"View Results Tree");
                resultCollector.setProperty(TestElement.ENABLED,"true");
                resultCollector.setProperty("ResultCollector.error_logging",false);

                threadGroupHashTree.add(resultCollector);
                // save generated test plan to JMeter's .jmx file format
                SaveService.saveTree(testPlanTree, new FileOutputStream(jmeterHome + slash + "example.jmx"));

                //add Summarizer output to get test progress in stdout like:
                // summary =      2 in   1.3s =    1.5/s Avg:   631 Min:   290 Max:   973 Err:     0 (0.00%)
                Summariser summer = null;
                String summariserName = JMeterUtils.getPropDefault("summariser.name", "summary");
                if (summariserName.length() > 0) {
                    summer = new Summariser(summariserName);
                }


                // Store execution results into a .jtl file
                String logFile = jmeterHome + slash + "example.jtl";
                ResultCollector logger = new ResultCollector(summer);
                logger.setFilename(logFile);
                testPlanTree.add(testPlanTree.getArray()[0], logger);

                // Run Test Plan
                //jmeter.configure(testPlanTree);
                //jmeter.run();

                System.out.println("Test completed. See " + jmeterHome + slash + "example.jtl file for results");
                System.out.println("JMeter .jmx script is available at " + jmeterHome + slash + "example.jmx");
                System.exit(0);

            }
        }

        System.err.println("jmeter.home property is not set or pointing to incorrect location");
        System.exit(1);


    }

    public static Object generateSampler(String name,String protocol,String domain,String path,String methodType,boolean followRedirects,boolean setPostBodyRaw,String jsonBody,HeaderManager headerManager,JSONPostProcessor jsonPostProcessor ){
        HTTPSamplerProxy examplecomSampler = new HTTPSamplerProxy();
        examplecomSampler.setProtocol(protocol);
        examplecomSampler.setDomain(domain);
        //examplecomSampler.setPort(80);
        examplecomSampler.setPath(path);
        examplecomSampler.setMethod(methodType);
        examplecomSampler.setName(name);
        examplecomSampler.setProperty(TestElement.TEST_CLASS, HTTPSamplerProxy.class.getName());
        examplecomSampler.setProperty(TestElement.GUI_CLASS, HttpTestSampleGui.class.getName());
        examplecomSampler.setFollowRedirects(followRedirects);
        examplecomSampler.setAutoRedirects(false);
        examplecomSampler.setUseKeepAlive(true);
        examplecomSampler.setDoMultipartPost(false);
        examplecomSampler.setPostBodyRaw(setPostBodyRaw);
        if (setPostBodyRaw) {
            examplecomSampler.addNonEncodedArgument("body", jsonBody, "");
        }
        HashTree requestHashTree = new HashTree();
        if (jsonPostProcessor!=null){
            requestHashTree.add(jsonPostProcessor);
        }
        if (headerManager!=null) {
            requestHashTree.add(examplecomSampler, headerManager);

            return  requestHashTree;
        }else{
            return examplecomSampler;
        }
    }

    public static HeaderManager getHeaderManagerObject(String headerManagerName,HashMap<String,Object> headerManagerHmap){
        HeaderManager headerManager=new HeaderManager();
        headerManager.setName(headerManagerName);

        Set<String> keys=headerManagerHmap.keySet();
        Iterator<String> iterator=keys.iterator();
        String key;
        while (iterator.hasNext()){
            key=iterator.next();
            headerManager.add(new Header(key,(String) headerManagerHmap.get(key)));
        }
        headerManager.setProperty(TestElement.TEST_CLASS,HeaderManager.class.getName());
        headerManager.setProperty(TestElement.GUI_CLASS, HeaderPanel.class.getName());
        return headerManager;
    }

    public static JSONPostProcessor getJSONPostProcessor(String name,String nameOfCreatedVarible,String jsonPath){
        JSONPostProcessor jsonPostProcessor=new JSONPostProcessor();
        jsonPostProcessor.setProperty(TestElement.TEST_CLASS, JSONPostProcessor.class.getName());
        jsonPostProcessor.setProperty(TestElement.GUI_CLASS, JSONPostProcessorGui.class.getName());
        jsonPostProcessor.setName(name);
        jsonPostProcessor.setRefNames(nameOfCreatedVarible);
        jsonPostProcessor.setJsonPathExpressions(jsonPath);

        return jsonPostProcessor;
    }

    public static CSVDataSet getCSVDataSet(String name,String delimiter,String fileEncoding,String filePath,boolean ignoreFirstLine,boolean setQuotedData,boolean setRecycle,String setShareMode,String setVariableNames,boolean setStopThread){
        CSVDataSet csvDataSet=new CSVDataSet();

        csvDataSet.setName(name);
        csvDataSet.setProperty("ignoreFirstLine", ignoreFirstLine);
        csvDataSet.setProperty("filename",filePath);
        csvDataSet.setProperty("delimiter",delimiter);
        csvDataSet.setProperty("fileEncoding",fileEncoding);
        csvDataSet.setProperty("variableNames",setVariableNames);
        csvDataSet.setProperty("quotedData",setQuotedData);
        csvDataSet.setProperty("recycle",setRecycle);
        csvDataSet.setProperty("stopThread",setStopThread);
        csvDataSet.setProperty("shareMode",setShareMode);
        csvDataSet.setEnabled(true);
        csvDataSet.setProperty(TestElement.TEST_CLASS, CSVDataSet.class.getName());
        csvDataSet.setProperty(TestElement.GUI_CLASS, TestBeanGUI.class.getName());

        return csvDataSet;
    }

    public static IfController getIf(){
        IfController ifController=new IfController();
        ifController.setName("First If Loop");
        ifController.setEnabled(true);
        ifController.setProperty(TestElement.TEST_CLASS, IfController.class.getName());
        ifController.setProperty(TestElement.GUI_CLASS, IfControllerPanel.class.getName());
        return ifController;
    }
}

```
