# Day-0, Day-1 , Day -2 Configuration for Cloud Networks [1]

![alt configuration lifecycle](images/../../../images/config-day.png)
![alt complete picture](images/../../../images/e2e_config.png)


## Nephio Use case Documentation (Proposal) [2]
Follows O-RAN requirements related to LCM of non-RT RIC Platform , RApps and Near-RT RIC and XApps

- Management of Non-RT RIC RApps
  - Non-RT RIC is considered as part of SMO, hence the platform itself may not be the responsibility of Nephio. However during the slice orchestration process, Nephio controllers may have to choose the RApp and the set of XApps required for SLA assurance of the Embb slice.
- Management of Near-RT RIC Platform and XApps
- Support A1 interface for Policy management between non-RT and near-RT RIC Platforms
- Support for E2 i/f between near-RT RIC and the E2 nodes (CU-CP/CU-UP and DU) for data subscription, policy and control management functions.

[1]: https://www.techplayon.com/day-0-day-1-day-2-configuration-for-cloud-networks/#:~:text=The%20Day%2D1%20and%20Day,files%2C%20Helm%20Charts%20and%20Value "blog"

[2]: https://docs.google.com/document/d/1xI581NL-Zp-MAtHt7lfxJKQLJgcQQf8x9vpTQoUc4yI/edit# "nephio proposal"