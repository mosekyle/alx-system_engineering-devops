# 0x19. Postmortem
This project is a wonderful.

It is inevitable that any software system will eventually experience failures, which can arise from various factors such as bugs, traffic spikes, security issues, hardware failures, natural disasters, and human error. Failure is a normal part of the software development process, and it presents a valuable opportunity for learning and improvement. It is crucial for Software Engineers to analyze their mistakes and take proactive measures to prevent their recurrence. While failure itself is acceptable, encountering the same issue multiple times is not.

In the tech industry, postmortems serve as a widely utilized tool. Following an outage, the responsible team(s) draft a comprehensive summary with two primary objectives:

1. Providing accessible information to all employees within the company, enabling them to understand the cause of the outage. As outages often have significant impacts on the organization, it is essential for managers and executives to comprehend the event's implications and how it may affect their work.
2. Ensuring that the root cause(s) of the outage are identified and appropriate measures are taken to rectify the issue, preventing its recurrence.

For the purpose of this postmortem, I will be utilizing either a web stack debugging project issue that I have previously encountered or an outage incident that I have personally faced.

## Resources
[Apiumhub](https://apiumhub.com/tech-blog-barcelona/software-development-project-postmortem/)

<img src=./image.png width=50%>

# BooktifuL requests failure report
Last week, an unfortunate incident unfolded when users of the BooktifuL platform encountered a frustrating obstacle. All requests made on the platform routes resulted in the dreaded 500 Error, rendering the services inaccessible. Regrettably, a significant majority of our valued users, precisely 90%, were impacted by this disruption. Upon thorough investigation, it was determined that the root cause of this predicament was the unexpected failure of our master server, web-01.

## Timeline
The error came to our attention on Saturday, April 29th, at 12:00 hours (East Africa Time) when our vigilant Site Reliability Engineer, Mr. Kyle, noticed a lag in speed on the master server. In response, our dedicated team of engineers swiftly disconnected the master server, web-01, to conduct a comprehensive system analysis. To ensure uninterrupted service, all requests were seamlessly redirected to the client server, web-02.

Our diligent team worked tirelessly to diagnose and rectify the underlying issue. Countless hours were dedicated to identifying the root cause and implementing effective solutions. Through their unwavering commitment, the problem was successfully resolved by Sunday, April 30th, at 18:00 hours (EAT).

## Root cause and resolution
The BooktifuL platform relies on a setup consisting of two Ubuntu cloud servers. Initially, the master server, web-01, was responsible for serving all incoming requests. Unfortunately, it encountered a memory outage due to the overwhelming number of requests. This situation arose because, during a previous testing phase, the client server, web-02, was temporarily disconnected for testing purposes and was not reconnected to the load balancer afterward.

To address the issue, our team took prompt action. The master server was temporarily disconnected to perform memory clean-up and optimize its performance. Subsequently, it was reconnected to the load balancer, ensuring a balanced distribution of requests between both the master and client servers.

To achieve a fair distribution of incoming traffic, a round-robin algorithm was implemented. This algorithm evenly allocates requests to both the master and client servers, allowing them to handle an equal amount of load and prevent any individual server from becoming overwhelmed.

By implementing these measures, we have successfully resolved the issue and restored normal functioning to the BooktifuL platform. We apologize for any inconvenience caused by this temporary disruption and assure you that we have learned from this experience. We will continue to refine and enhance our system architecture to ensure optimal performance and a seamless user experience.

## Measures against such problem in future
Select the optimal load balancing algorithm that suits your program's requirements and traffic patterns.
Maintain continuous monitoring of your servers to ensure their smooth operation and promptly address any issues that arise.
 Implement additional backup servers as a precautionary measure to prevent your program from experiencing complete downtime during any unexpected issues.
