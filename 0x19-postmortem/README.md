Postmortem: Database Outage on E-Commerce Platform
Issue Summary
Duration of Outage: June 5, 2024, 14:00 UTC - June 5, 2024, 16:30 UTC
Impact: The database outage affected our e-commerce platform's checkout service. Users experienced inability to complete purchases. Approximately 75% of the users were impacted.
Root Cause: The root cause was a misconfiguration in the database replication settings, which led to a cascading failure when the primary database node failed.
Timeline
14:00 UTC: Issue detected by automated monitoring system (alert for high error rates on checkout service).
14:05 UTC: Engineers started investigating the checkout service, suspecting a load balancer issue.
14:15 UTC: Further investigation revealed that database queries were failing.
14:20 UTC: Database team was alerted and began investigating potential causes within the database.
14:35 UTC: Initial hypothesis was a network issue, leading to checks on network hardware and configuration.
15:00 UTC: Misleading paths: Focus was briefly on a potential DDoS attack.
15:15 UTC: Database logs analyzed, revealing replication errors and inconsistencies.
15:30 UTC: Incident escalated to senior database architect.
15:45 UTC: Primary node failure identified; misconfiguration in replication settings confirmed.
16:00 UTC: Failover to secondary database initiated; replication settings corrected.
16:30 UTC: System restored; checkout service fully functional.
Root Cause and Resolution
Root Cause:
The failure stemmed from a misconfiguration in the database replication settings. The replication lag was set too high, which caused a significant delay in propagating changes from the primary to the secondary node. When the primary node experienced a hardware failure, the failover to the secondary node did not occur as expected, leading to a complete outage of the checkout service.

Resolution:
The resolution involved immediate failover to the secondary database node and correcting the replication settings to ensure real-time data consistency. The replication lag settings were adjusted to a much lower threshold to facilitate quicker failover responses in future incidents. Additionally, a thorough check was conducted on all database nodes to ensure consistency and stability post-recovery.

Corrective and Preventative Measures
Improvements:

Replication Settings: Tighten replication lag settings to ensure near-instant failover capabilities.
Monitoring Enhancements: Implement more granular monitoring and alerting for database replication status and failover readiness.
Failover Procedures: Develop and document detailed failover procedures, including regular drills to ensure team readiness.
Task List:

Patch Database Servers: Apply latest patches to all database servers to prevent known issues.
Review and Adjust Replication Lag Settings: Ensure replication lag settings are optimized for minimal delay.
Enhance Monitoring: Add specific alerts for replication lag and failover status.
Regular Failover Drills: Schedule monthly failover drills to test and refine procedures.
Documentation: Update all relevant documentation to include new settings, procedures, and best practices learned from the outage.
Training: Conduct a training session for all engineers on the new failover procedures and the importance of replication settings.
By implementing these measures, we aim to prevent similar incidents in the future and ensure that our platform remains reliable and resilient to failures.
