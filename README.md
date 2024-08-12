# Lab 1 - Continuous Deploy - Frontend

### Summary: Setup a CD pipeline to take an artifact deploy it to an environment

**Learning Objective(s):**

- Configure a basic pipeline using Harness CD

- Create a k8s service

- Incorporate an advanced deployment strategy such as Canary

- Create custom Harness variables

- Create an Input Set

**Steps**

1. From the left hand menu, navigate to **Projects** → **Select the project available**\
   ![](https://lh7-us.googleusercontent.com/docsz/AD_4nXfhuMykMsIHl-7FjliWssHc0uwRpdLdrnq7GkGAI0g6UBZM69F1zpQ8ZA8N_vMqjpoGFYFR_weJk7OtOGGa2bksIaS6BlktwytmuJ1THM3e8O6tDT18HYWwFyGUye8ubsrHBChI8ORrCQ88JcKWpLjQ0DsXDS0NSZrkfZ4RUQ?key=cRG2cvp_PHVW0KG2Gq6Y_A)

1) From the left hand side menu select **Pipelines**

2) Click **+ Create a Pipeline**, enter the following values, then click **Start**

| Field                                  | Value            | Notes
| -------------------------------------- | ---------------- | ------------------------------------------------------------------------------------------ |
| Name                                   |workshop|                                                                                            |
| How do you want to setup your pipeline |Inline| This indicates that Harness (rather than Git) will be the source of truth for the pipeline |
                                                                                   
2. From Pipeline Studio, add a Deployment stage by clicking **Add Stage** and select **Deploy** as the Stage Type

3. Enter the following values and click on **Set Up Stage**


| Input           | Value          | Notes |
| --------------- | -------------- | ----- |
| Stage Name      |frontend|       |
| Deployment Type |Kubernetes|       |

4. Configure the **frontend** Stage with the following\
   **Service**

- Click **+Add Service** and configure as follows****


| Input                      | Value                                               | Notes                              |
| -------------------------- | --------------------------------------------------- | ---------------------------------- |
| Name                       |frontend|                                    |
| Deployment Type            |Kubernetes|                                    |
| * **Add Manifest**         |                                                     |                                    |`
| Manifest Type              |K8s Manifest|                                    |
| K8s Manifest Store         |Code|                                    |
| Manifest Identifier        |templates|                                    |
| Repository                 |harnessrepo|                                    |
| Branch                     |main|                                    |
| File/Folder Path           |harness-deploy/frontend/manifests|                                    |
| Values.yaml                |harness-deploy/frontend/values.yaml|                                    |
| - **Add Artifact Source**  |                                                     |                                    |
| Artifact Repository Type   |Docker Registry|                                    |
| Docker Registry Connector  |dockerhub|                                    |
| Artifact Source Identifier |frontend|                                    |
| Image Path                 |nikpap/harness-workshop|                                    |
| Tag                        |<+variable.org.frontendtag>| _Switch to expression and set to_  |

**Environment**

The target infrastructure has been pre-created for us. The application will be deployed to a k8s cluster on the given namespace  

- Click **- Select -** on the environment input box ****

- Select **prod** environment****

| Input | Value | Notes                                                             |
| ----- | ----- | ----------------------------------------------------------------- |
| Name  |prod| Make sure to select the environment and infrastructure definition |

- Click **- Select -** on the environment input box ****

-  From the dropdown select k8s

| Input | Value | Notes |
| ----- | ----- | ----- |
| Name  |k8s|       |

**Execution**

- Select **Rolling** and click on **Use Strategy**, the frontend is a static application so no need to do canary, new features will be managed by Feature Flags at a later stage of this lab


# Lab 2 - Continuous Deploy - Backend

### Summary: Extend your existing pipeline to derisk production deployments

**Learning Objective(s):**

- Utilise complex deployment strategies to reduce blast radius of a release 

**Steps**

5. In the existing pipeline, add a Deployment stage by clicking **Add Stage** and select **Deploy** as the Stage Type

6. Enter the following values and click on **Set Up Stage**


| Input           | Value          | Notes |
| --------------- | -------------- | ----- |
| Stage Name      |backend|       |
| Deployment Type |Kubernetes|       |

7. Configure the **backend** Stage with the following\
   **Service**

- Click **Select Service** and configure as follows****


| Input | Value       | Notes |
| ----- | ----------- | ----- |
| Name  |backend|       |

**Environment**

The target infrastructure has been pre-created for us and we used it in the previous stage. To reuse the same environment

- Click **- Propagate Environment From**

- Select **Stage \[frontend]**

**Execution**

- Select **Canary**  and click on **Use Strategy**

8. Click **Save** and then click **Run** to execute the pipeline with the following inputs. As a bonus, save your inputs as an Input Set before executing (see below)

| Input       | Value | Notes       |
| ----------- | ----- | ----------- |
| Branch Name |main| Leave as is |

9. While the canary deployment is ongoing navigate to the web page and see if you can spot the canary (use the check release button) 

| project                | domain        | suffix |
| ---------------------- | ------------- | ------ |
|http\://project_id|.cie-bootcamp|.co.uk|

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXfmb1N3lAe0EOnEun9neU9y3ilqy3HbxfnWfUMzF3FsykslwgQfU_W4pE0wlt5kYSp6_mTs7cVP0anhJ7uvtsytal2qX3ZEq3vvOT3DOBUzE9SZ3rpwkAHP6e_ExdRbo5VmN2kpxdFlp6u8iGaKwhW_uyAohEmJurkjmEB2Ww?key=cRG2cvp_PHVW0KG2Gq6Y_A)

# Lab 3 - Feature Flags

### Summary: Build and deploy your first feature flag 

**Learning Objective(s):**

- Create a Feature Flag

- Create an SDK key

- Deploy application that uses Flag/SDK Key

- Toggle Feature Flag to enable/disable feature

**Steps**

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXdxbh_5hgTG2CsE8Dp_5_BLB75OITfS-9xxW-xplPehdYbj38WMTloCOo4tbOAom9VRc65S99IB54w-TY7INiG6Bd8PMqvRs_EsTQHzKjCZTjnv8laP7XCEuf9_l3s8HV3UuxVsnTgzuZpkV6Fq-FVoqpHY5kSuQ3un7Xrssg?key=cRG2cvp_PHVW0KG2Gq6Y_A)

**Create the SDK Key**  

1. From the left hand side menu under Feature Flags,  select **environments**

2. From the list select the prod environment

3. Click **+ New SDK Key**, configure as follows and click **Create**

| Input    | Value      | Notes |
| -------- | ------     | ----- |
| Name     |sdk|       |
| Key Type |client|       |

4. Copy the secret to use later. Note that the key will be redacted once you leave the page.

5. From the left hand side menu select Project settings

6. From the resources available click on the **Variables** 

7. Modify the sdk variable and copy in the key

| Input | Value                               | Notes |
| ----- | ----------------------------------- | ----- |
| Name  |sdk|       |
| Value | _SDK Key copied from previous step_ |       |

4. Click **Save**

********

**Create the Flag**

1. From the left hand menu, go to **Feature Flags** → **Feature Flags**

2. Click **+ New Feature Flag,** configure as follows and click **Save and Close**.

| Input                         | Value      | Notes |
| ----------------------------- | --------------   | ----- |
| Type                          |Boolean|       |
| Name                          |webinarff|       |
| **Variation Settings**        |                  |       |
| Name (first one)              |Show Offer|       |
| Name (second one)             |Hide Offer|       |
| If the flag is Enabled, serve |Show Offer|       |

3. Enable the flag by clicking on the **Flag is Disabled** button and click **Save**

4. **Run** the pipeline created in previous steps

**Change the Flag via the UI**

1. From the left hand menu in Harness, go to **Feature Flags** → **Target Management**

2. Select the target shown in the list. If target is not shown, create the target manually

| Input      | Value     | Notes |
| ---------- | --------- | ----- |
| Name       |webinar|       |
| Identifier |webinar|       |

3. Click **Add Flag**, toggle **webinarff**, set the variation to **Show Offer**, then click on **Add 1 Flags**

4. Note that your application now displays a special offer

5. For your target, set the variation to **Hide Offer** and click **Save Chances**

6. Note that your application now does NOT display the special offer

# Lab 4 - Governance/Policy as Code

### Summary: Create and apply policies as code in order to enable governance and promote self-service

**Learning Objective(s):**

- Create a policy that evaluates when editing pipelines

- Create a policy that evaluates during pipeline execution

- Test policy enforcement

**Steps**:

**Create a Policy to require Approvals**

1. From the secondary menu, expand **Project Setup** and select **Governance Policies**

2. Click **Build a Sample Policy**

3. From the suggested list select **Pipeline - Approval**  and click on next

4. Click Next: Enforce Policy

5. Set the values according to the table  below and confirm

| Input            | Value        | Notes |
| ---------------- | ------------ | ----- |
| Trigger Event    |On Run|       |
| Failure Strategy |Error & exit|       |

**Test the Policy to require Approvals**

1. Open your pipeline

2. Try to run the pipeline and note that the failure due to lack of an approval stage

3. Open the pipeline in edit mode and navigate to the “**frontend**” stage

4. Before the Rolling Deployment step add an **Harness Approval** step according to the table below

| Input            | Value            | Notes |
| ---------------- | ---------------- | ----- |
| Step Name        |Approval|       |
| Type of Approval |Harness Approval|       |

5. Configure the Approval step as follows

| Input       | Value             | Notes |
| ----------- | ----------------- | ----- |
| Name        |Approval|       |
| User Groups |All Project Users|       |

6. Repeat for the **backend** stage

7. Click **Save**

8. Try to **Run** the pipeline and note that you can succseefully run the pipeline without any policy failure 

# (BONUS) Lab 5 - Continuous Verification

### Summary: Automate the verification of new releases 

**Learning Objective(s):**

- Add continuous verification to the deployed service

- Automate release validation

**Steps**

1. In the existing pipeline, within the Deploy backend stage **after** Canary Deployment step click on the plus icon to add a new step

2. Add a **Verify** step with the following configuration

| Input                        | Value  | Notes                                                                                            |
| ---------------------------- | ------ | ------------------------------------------------------------------------------------------------ |
| Name                         |Verify|                                                                                                  |
| Continuous Verification Type |Canary|                                                                                                  |
| Sensitivity                  |Low| This is to define how sensitive the ML algorithms are going to be on deviation from the baseline |
| Duration                     |5mins|                                                                                                  |

Click **Save** and then click **Run** to execute the pipeline with the following inputs. As a bonus, save your inputs as an Input Set before executing (see below)

| Input       | Value | Notes       |
| ----------- | ----- | ----------- |
| Branch Name |main| Leave as is |

# (BONUS) Lab 6 - Validate Release

**Learning Objective(s):**

- Identify the difference in traffic between normal and canary instances of the application

- Automate release validation

- Use complex deployment strategies to reduce the blast radius

**Steps**:

- While the canary deployment is ongoing navigate to the web page and see if you can spot the canary (use the check release button) 

| project                | domain        | suffix |
| ---------------------- | ------------- | ------ |
| http\://\<project\_id>|.cie-bootcamp|.co.uk|

- Drill down to the distribution test tab and run the traffic generation by clicking the **Play** button

- Observe the traffic distribution

- Validate the outcome of the verification on the pipeline execution details

\
![](https://lh7-us.googleusercontent.com/docsz/AD_4nXdbAmEJ5zQPsKlw_nEknWvYo97pm5eWCXr6vU8-GgIL0ulAOSH9N07PoEcVSknARVQo7Tgj1s31VHqR1I3hu2dMIO1rIX5HHcmTPXoQPoyo8CPv13OhnJN5WVcZqSwUXzdDHmm3PxUnhtpGVl0PAMJ_1wnuodvUbVPBOdnGKQ?key=cRG2cvp_PHVW0KG2Gq6Y_A)
![](https://lh7-us.googleusercontent.com/docsz/AD_4nXf-5oWX9OfvdmEb9MBm2_h2KKAa_QwmiJoM0fiKrTuxAr6GR4wxeulSlk48gyBK3dykrtIslDSkxpiGytrxH0JaxaQ4ZgTYxbmc8OenAH3nhGCvvOAxkWVjVBp1TRg_qQQi9z8OrNPK4udPtNL1LIyym6Ch5IMzrulFOcXhOQ?key=cRG2cvp_PHVW0KG2Gq6Y_A)

**Bonus**:

- Add a canary rollout from 10% to 50% traffic and see how this impacts the traffic distribution
