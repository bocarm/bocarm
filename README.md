package org.fog.test.perfeval;
import java.lang.ModuleLayer.Controller;
import java.util.ArrayList;
import java.util.Calendar;
import java.util.List;
import org.cloudbus.cloudsim.Log;
import org.cloudbus.cloudsim.core.CloudSim;
import org.fog.application.AppEdge;
import org.fog.application.AppLoop;
import org.fog.application.Application;
import org.fog.application.selectivity.FractionalSelectivity;
import org.fog.entities.Actuator;
import org.fog.entities.FogBroker;
import org.fog.entities.FogDevice;
import org.fog.entities.Sensor;
import org.fog.entities.Tuple;
import org.fog.placement.ModuleMapping;
import org.fog.placement.ModulePlacementEdgewards;
import org.fog.placement.ModulePlacementMapping;
import org.fog.utils.TimeKeeper;
import org.fog.utils.distribution.DeterministicDistribution;
public class FogM {
	//Create the list of fog devices
	static List<FogDevice> fogDevices = new ArrayList<FogDevice>();
	//Create the list of sensors
	static List<Sensor> sensors = new ArrayList<Sensor>();
	//Create the list of actuators
	static List<Actuator> actuators = new ArrayList<Actuator>();
	//Define the number of fog nodes will be deployed
	static int numOfFogDevices = 10;
	//Define the number of gas sensors with each fog nodes
	static int numOfGasSensorsPerArea=1;
	//Define the number of chemical sensors with each fog nodes
	static int numOfChSensorsPerArea=1;
	//Define the number of surrounding sensors with each fog nodes
	static int numOfSrSensorsPerArea=1;
	//We are using the fog nodes to perform the operations.
	//cloud is set to false
	private static boolean CLOUD = false;
	private static java.lang.ModuleLayer.Controller Null;
	@SuppressWarnings("unused")
	public static <dependency, artifactId> void main(String[]args ) {
	Log.printLine("Waste Management system...");
	try {
	Log.disable();
	int num_user = 1; // number of cloud users
	Calendar calendar = Calendar.getInstance();
	boolean trace_flag = false; // mean trace events
	CloudSim.init(num_user, calendar, trace_flag);
	String appId = "mins"; // identifier of the application
	FogBroker broker = new FogBroker("broker");
	Application application =
	createApplication(appId, broker.getId());
	application.setUserId(broker.getId());
	createFogDevices(broker.getId(), appId);
	Controller Controller = Null;
	
	// initializing a module mapping
	ModuleMapping moduleMapping =
	ModuleMapping.createModuleMapping();
	for(FogDevice device : fogDevices){
	if(device.getName().startsWith("a")){
	moduleMapping.addModuleToDevice("master-module",
	device.getName());
	if(device.getName().startsWith("g")){
	moduleMapping.addModuleToDevice("gasinfo-module",
	device.getName()); }
	if(device.getName().startsWith("c")){
	moduleMapping.addModuleToDevice
	("chemicalinfo-module", device.getName()); }
	if(device.getName().startsWith("s")){
	moduleMapping.addModuleToDevice
	("srinfo-module", device.getName());
	}}}
	// if the mode of deployment is cloud-based
	if(CLOUD)
	{
	// placing all instances of master-module in Cloud
	addModuleToDevice("mastermodule", "cloud");
	moduleMapping.addModuleToDevice("gasinfo-module", "cloud");
	moduleMapping.addModuleToDevice("chinfo-module", "cloud");
	moduleMapping.addModuleToDevice("srinfo-module", "cloud");
	}


	Controller = Controller("master-controller", fogDevices, sensors, actuators);
	Controller = submitApplication(application, (CLOUD)?(new ModulePlacementMapping(fogDevices, application,
	moduleMapping))
	:(new ModulePlacementEdgewards(fogDevices, sensors, actuators,
	application, moduleMapping)));
	TimeKeeper.getInstance().setSimulationStartTime(
	Calendar.getInstance().
	getTimeInMillis());
	CloudSim.startSimulation();
	CloudSim.stopSimulation();
	Log.printLine("mining industry simulation finished!");
	} catch (Exception e) {
	e.printStackTrace();
	Log.printLine("Unwanted errors happen");
	}
	}
	private static java.lang.ModuleLayer.Controller submitApplication(Application application, Object object) {
		return null;
		// TODO Auto-generated method stub
		
	}
	private static Controller Controller(java.lang.String string2, List<FogDevice> fogDevices2, List<Sensor> sensors2,
			List<Actuator> actuators2) {
		// TODO Auto-generated method stub
		return null;
	}
	public FogM(){}
	@SuppressWarnings("unused")
	private static void publicFogM() {
		// TODO Auto-generated method stub
		
	}

	private static void addModuleToDevice(String string, String string2) {
		// TODO Auto-generated method stub
		
	}

	private static void createFogDevices(int userId, String appId) {
		FogDevice cloud = createFogDevice("cloud", 44800, 40000, 100,
		10000, 0, 0.01, 16*103,
		16*83.25);

		fogDevices.add(cloud);
		FogDevice router = createFogDevice("proxy-server", 7000, 4000,
		10000, 10000, 1, 0.0, 107.339, 83.4333);	
		router.setParentId(cloud.getId());
		router.setUplinkLatency(100.0);
		fogDevices.add(router);
		for(int i=0;i<numOfFogDevices;i++){
		addFogNode(i+"", userId, appId, router.getId());
		}
		}
		private static FogDevice createFogDevice(String string, int i, int j, int k, int l, int m, double d, double e,
			double e2) {
		// TODO Auto-generated method stub
		return null;
	}

		private static FogDevice createFogDevice(String string, int i, int j, int k, int l, int m, double d, int n,
			double e) {
		// TODO Auto-generated method stub
		return null;
	}

		private static FogDevice addFogNode(String id,
		int userId, String appId, int parentId)
		{
		FogDevice fognode = createFogDevice("a-"+id, 5000, 4000, 10000,
		10000, 3, 0.0, (int) 107.339, 83.4333);
		fogDevices.add( fognode);
		fognode.setUplinkLatency(1.0);
		for(int i=0;i<numOfGasSensorsPerArea;i++){
		addGasSensors(i+"", userId, appId, fognode.getId());
		}
		for(int i=0;i<numOfChSensorsPerArea;i++){
		addChSensors(i+"", userId, appId, fognode.getId());
		}
		for(int i=0;i<numOfSrSensorsPerArea;i++){
			addSrSensors(i+"", userId, appId, fognode.getId());
		}
		return fognode;
		}
		private static FogDevice addGasSensors(String id, int userId,
		String appId, int parentId){
		FogDevice gasSensor = createFogDevice("g-"+id, 5000, 1000, 10000,
		10000, 4, 0, (int) 87.53, 82.44);
		gasSensor.setParentId(parentId);
		Sensor sensor = new Sensor("s-"+id, "GAS", userId, appId, new
		DeterministicDistribution(5));
		sensors.add(sensor);
		Actuator ptz = new Actuator("act-"+id, userId,
		appId, "ACT_CONTROL");
		actuators.add(ptz);
		sensor.setGatewayDeviceId(gasSensor.getId());
		sensor.setLatency(1.0);
		ptz.setGatewayDeviceId(parentId);
		ptz.setLatency(1.0);
		return gasSensor;
		}
		private static FogDevice addChSensors(String id, int userId,
		String appId, int parentId){
		FogDevice chSensor = createFogDevice("c-"+id, 5000, 1000, 10000,
		10000, 4, 0, 87.53, 82.44);
		chSensor.setParentId(parentId);
		Sensor sensor = new Sensor("sch-"+id, "CH", userId, appId,
		new DeterministicDistribution(5));
		sensors.add(sensor);
		Actuator ptzch = new Actuator("actch-"+id, userId,
		appId, "ACT_CONTROLCH");
		actuators.add(ptzch);
		sensor.setGatewayDeviceId(chSensor.getId());
		sensor.setLatency(1.0);
		ptzch.setGatewayDeviceId(parentId);
		ptzch.setLatency(1.0);
		return chSensor;
		}
		private static FogDevice addSrSensors(String id, int userId,
		String appId, int parentId){
		FogDevice srSensor = createFogDevice("s-"+id, 5000, 1000, 10000,
		10000, 4, 0, (int) 87.53, 82.44);
		srSensor.setParentId(parentId);
		Sensor sensor = new Sensor("ssr-"+id, "SR", userId, appId,
		new DeterministicDistribution(5));
		sensors.add(sensor);
		Actuator ptzch = new Actuator("actsr-"+id, userId,
		appId, "ACT_CONTROLSR");
		actuators.add(ptzch);
		sensor.setGatewayDeviceId(srSensor.getId());
		sensor.setLatency(1.0);
		ptzch.setGatewayDeviceId(parentId);
		ptzch.setLatency(1.0);
		return srSensor;
		}
		@SuppressWarnings("serial")
		private static Application createApplication(String appId,
				int userId){
				Application application = Application.createApplication(appId,
				userId);
				application.addAppModule("gasinfo-module", 10);
				application.addAppModule("master-module", 10);
				application.addAppModule("chinfo-module", 10);
				application.addAppModule("srinfo-module", 10);
				application.addAppEdge("GAS", "master-module",1000,
				2000, "GAS", Tuple.UP, AppEdge.SENSOR);
				application.addAppEdge("CH", "chinfo-module", 1000,
				2000, "CH", Tuple.UP, AppEdge.SENSOR);
				application.addAppEdge("SR", "srinfo-module", 1000, 2000,
				"SR",
				Tuple.UP, AppEdge.SENSOR);
				application.addAppEdge("master-module", "gasinfo-module",
				1000, 2000,
				"gasTask", Tuple.UP, AppEdge.MODULE);
				application.addAppEdge("master-module", "chinfo-module", 1000,
				2000, "chTask", Tuple.UP, AppEdge.MODULE);
				application.addAppEdge("master-module", "srinfo-module", 1000,
				2000, "srTask", Tuple.UP, AppEdge.MODULE);
				//Response
				application.addAppEdge("gasinfo-module", "master-module",
				1000, 2000, "gasResponse", Tuple.UP, AppEdge.MODULE);
				application.addAppEdge("chinfo-module", "master-module",
				1000, 2000, "chResponse", Tuple.UP, AppEdge.MODULE);
				application.addAppEdge("srinfo-module", "master-module",
				1000, 2000,"srResponse", Tuple.UP, AppEdge.MODULE);
				application.addTupleMapping("master-module", "GAS", "gasTask",
				new FractionalSelectivity(1.0));
				application.addTupleMapping("master-module", "CH", "chTask",
						new FractionalSelectivity(1.0));
				application.addTupleMapping("master-module", "SR", "srTas",
				new FractionalSelectivity(1.0));
				application.addTupleMapping("gasinfo-module",
				"gasTask", "gasResponse",
				new FractionalSelectivity(1.0));
				application.addTupleMapping("chinfo-module",
				"chTask", "chResponse",
				new FractionalSelectivity(1.0));
				application.addTupleMapping("srinfo-module",
				"srTask", "srResponse",
				new FractionalSelectivity(1.0));
				final AppLoop loop1 = new AppLoop(new ArrayList<String>()
				{{add("GAS");
				add("master-module");add("gasinfo-module");add("gasTask");
				add("gasResponse");}});
				final AppLoop loop2 = new AppLoop(new
				ArrayList<String>(){{add("CH");
				add("master-module");add("chinfo-module");
				add("chTask")
				;add("chResponse");}});
				final AppLoop loop3 = new AppLoop(new
				ArrayList<String>(){{add("SR");
				add("master-module");add("srinfo-module")
				;add("srTask");add("srResponse");}});
				List<AppLoop> loops = new
				ArrayList<AppLoop>(){{add(loop1);add(loop2)
				;add(loop3);}};
				application.setLoops(loops);
				return application;
				}
		
}
- 👋 Hi, I’m @bocarm
- 👀 I’m interested in  le langa javad'application de l'industrie minière intelligente basé sur le brouillard d'iFogSim.
- 🌱 I’m currently learning  le java 
- 💞️ I’m looking to collaborate on  un  problém  que je rencontre  sur mon aplllication java  sur l'IOT
Merci


java.lang.NullPointerException: Cannot invoke "org.fog.entities.FogDevice.getId()" because "cloud" is null
	at org.fog.test.perfeval.FogM.createFogDevices(FogM.java:132)
	at org.fog.test.perfeval.FogM.main(FogM.java:55)

- 📫 How to reach me ...

<!---
bocarm/bocarm is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
