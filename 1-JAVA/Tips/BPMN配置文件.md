## Activiti工作流

* 工作流：是将一组任务组织起来以完成某个经营过程：定义了任务的触发顺序和触发条件，每个任务可以由一个或多个软件系统完成，也可以由一个或一组人完成，还可以由一个或多个人与软件系统协作完成。
* 工作流管理系统的目标：管理工作的流程以确保工作在正确的时间被期望的人员所执行---在自动化进行的业务过程中插入任何的执行和干预。

### [BPMN配置文件](https://blog.csdn.net/qq_30739519/article/details/51271580)

* bpmn文件是activiti配置流程定义的文件，一般一个bpmn文件定义一个流程，文件为xml格式

* 各种元素级别

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <definitions>	
      <process>
      process：流程定义根元素，代表一个流程定义的开始
          id：流程唯一id，启动流程时需要
          isExecutable：流程是否可执行
          name：流程名称
          type：流程类型
          isClosed：流程是否已关闭，关闭不能执行
          <startEvent>
          startEvent：流程启动事件,一个process只能有一个,且必须为流程起始元素
          	id：启动节点id
          	name：启动节点名称
          </startEvent>
          <endEvent>
          endEvent: 流程结束事件,一个process只能有一个,且必须为流程结束元素
              id：结束节点id
  			name：节点名称
          </endEvent>
          <userTask>
          userTask: 流程中间用户任务,夹在startEvent与endEvent之间的节点
              id：任务id,使用id操作任务
              name：任务名称
              activiti:assignee：任务所属用户,只能指定用户完成这个任务,即任务办理人
              activiti:candidateUsers：多个任务办理人
              activiti:candidateGroups：任务处理人候选组,处理人必须在这个组内
              activiti:exclusive：独家的,好像是在排它性网关中使用,意思应该是在有并行分支情况下,只会走其中一条
             	activiti:dueDate：设置用户任务到期日期
              activiti:priority：用户任务优先级,0-100
              <extensionElements>
              extensionElements: userTask的子元素,用于扩展元素
                  <activiti:taskListener>
                  activiti:taskListener: 扩展元素之一,用于监听某个任务的运行
                      event：监听的任务事件名,create、assignment（分配任务）、complete
                      class：任务监听器类,需要实现TaskListener
                  </activiti:taskListener>
              </extensionElements>
          </userTask>
          <scriptTask/>
          <manualTask/>
          <receiveTask/>
          <serviceTask/>
          <businessRuleTask/>
          <exclusiveGateway/>
          exclusiveGateway: 排它性网关,即多个sequenceFlow以网关节点开始时,只根据条件执行其中一条流,其他流不再判断
  		虽然与userTask同属于节点,但是其不作为任务执行
          	id：	节点id
          	name：	节点名称
          	gatewayDirection：网关方向,Unspecified
          <parallelGateway/>
          <sequenceFlow>
          sequenceFlow: 顺序流分为两种：标准顺序流  条件顺序流,其实就是连接两个节点的一条线
              id：顺序流id
              sourceRef：连线的起始节点id,即接近startEvent的节点
              targetRef：连线结束节点id,即接近endEvent的节点
              <conditionExpression>
              conditionExpression: sequenceFlow子元素,根据表达式确定是否执行这一顺序流,一条顺序流只能联系两个节点
              如果需要表达式判断,有多条顺序流连接了同一开始节点,一般这样的开始节点都是网关
                 xsi:type： 含义不知道,值为tFormalExpression
                 子元素：	表达式,${days <= 3}
              </conditionExpression>
          </sequenceFlow>
          <subProcess></subProcess>
          <boundaryEvent/></boundaryEvent>
      </process>
  <bpmndi:BPMNDiagram>
      <bpmndi:BPMNPlane>
          <bpmndi:BPMNShape>
              <omgdc:Bounds></omgdc:Bounds>
          </bpmndi:BPMNShape>
          <bpmndi:BPMNShape>
              <omgdc:Bounds></omgdc:Bounds>
          </bpmndi:BPMNShape>
          <bpmndi:BPMNShape>
              <omgdc:Bounds></omgdc:Bounds>
          </bpmndi:BPMNShape>
          <bpmndi:BPMNShape>
              <omgdc:Bounds></omgdc:Bounds>
          </bpmndi:BPMNShape>
          <bpmndi:BPMNEdge>
              <omgdi:waypoint></omgdi:waypoint>
              <omgdi:waypoint></omgdi:waypoint>
          </bpmndi:BPMNEdge>
      </bpmndi:BPMNPlane>
  </bpmndi:BPMNDiagram>
  </definitions>
  ```

---

