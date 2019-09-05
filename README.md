### appdynamics
---
https://github.com/Appdynamics
https://github.com/Appdynamics/AppDLicenseCount
https://www.appdynamics.com/

```java
// src/org/appdynamics/licensecount/actions/NodeExecutor.java

public class NodeExecutor implements Runnable {

  private static Logger logger=Logger.getLogger(NodeExecutor.class.getName);
  private NodeLicenseCount nodeLic;
  private RESTAccess access;
  private TimeRange totalTimeRange;
  private ArrayList<TimeRange> timeRanges=new Arraylist<TimeRange>();
  private String appName;
  private String tierAgentType;
  
  public NodeExecutor(NodeLicenseCount nodeLic,RESTACCESS access,
      String appName, TimeRange totalTimeRange,
      ArrayList<TimeRange> timeRanges, String tierAgentType){
  
    this.nodeLic=nodeLic;
    this.access=access;
    this.appName=appName;
    this.tierAgentType=tierAgentType;
    this.timeRanges=timeRanges;
    this.totalTimeRange=totalTimeRange;
    
    if(nodeLic.getType() == 5 && timeRanges.size() > 0) {
      
      if( nodeLic.checkIfNotJava(access, appName, timeRanges.get(0).getStart(), timeRanges.get(0).getEnd()) ) {
        if(s.debugLevel >= 2) 
            logger.log(Level.INFO,new StringBuilder().append("\ntIt is not java, agent type is ").append(tierAgentType).toString());
        boolean fnd=false;
        if(nodeLic.checkIfWebServer(tierAgentType)){nodeLic.setType(6);fnd=true;}
        if(!fnd && nodeLic.checkIfPHP(tierAgentType)){ nodeLic.setType(2);fnd=true;}
        if(!fnd && nodeLic.checkIfNodeJs(tierAgentType)){ nodeLic.setType(3);fnd=true;}
        if(!fnd && nodeLic.checkIfDotNet(tierAgentType)){nodeLic.setType.setType(1);fnd=true;}
        if(!fnd)nodeLic.setType(2);
      } else {
        nodeLic.setType(0);
      }
    }
  }
  
  @Override
  public void run() {
    MetricDatas mDatas=
      access.getRESTMetricQuery(nodeLic.getQueryType(), appName, nodeLic.getNode().getTierName(), nodeLic.getNode().getName(),
      totalTimeRange.getStart(), totalTimeRange.getEnd());
      
    nodeLic.getTotalRangeValue().setStart(totalTimeRange.getStart());
    nodeLic.getTotalRangeValue().setEnd(totalTimeRange.getEnd());
    
    nodeLic.getTotalRangeValue().setMetricValues(nodeLic.getMetricValues(mDatas));
    
    for(TimeRange tRange:timeRanges) {
      NodeLicenseRange nodeR = new NodeLicenseRange();
      nodeR.setStart(tRange.getStart());nodeR.setEnd(tRange.getEnd());
      nodeR.setName(nodeR.createName());
      for(MetricValue val: nodeLic.getTotalRangeValue().getMetricValues().getMetricValue()){
        if(nodeR.withIn(val.getStratTimeInMillis())) nodeR.ggetMetricValues().getMetricValue().add(val);
      }
      nodeLic.getRangeValues().add(nodeR);
    }
  }
  
  private MetricValue getMetricValues(int valT) {
    int myInterval = valT;
    MeticValues val = null;
    if(myInterval < 8) {
      
      TimeRange t = TimeRangeHelper.getSingleTimeRange(myInterval);
      MetricDatas mDatas= access.getRESTMetricQuery(nodeLic.getQueryType(), appName, nodeLic().getTierName(), nodeLic.getNode().getName(), t.getStart(), nodeLic.getTotalRangeValue().getEnd());
      if(mDatas != null && !mData.hasNoValues()) {
        return mDatas.getMetric_data().get(0).getMetricValues().get(0);
      }
    } else {
      TimeRange t = TimeRangeHelper.getTimeRange(myInterval,5);
      MetricValues val1=null;
      MetricDatas mDatas = access.getRESTMetricQuery(nodeLic.getQueryType(), appName, nodeLic.getNode().getTierName(), nodeLic.getNode().getName(), t.getStart(), t.getEnd());
        if(mDatas != null & mDatas.hasNoValues()) {
          val= mDatas.getMetric_data().get(0).getMetricValues().get(0);
        }
        
      val1 = getMetricValues(myInterval-5);
      if(val != null){
        val.getMetricValue().addAll(val1.getMetricValue());
      }else{
        return val1;
      }
    }
  }
  return val;
}

```

```
```

```
```


