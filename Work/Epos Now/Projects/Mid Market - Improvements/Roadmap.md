#### 1. **Project Planning and Requirement Gathering**

- **Understand the Requirements**: Review the project specifications in detail.
- **Identify Stakeholders**: Ensure you have a list of all stakeholders, including the AMs (Adam Boast, Paul Beck, Charles Holgate) and any new team members in APAC and USA.
- **Schedule Meetings**: Arrange initial and follow-up meetings with stakeholders to clarify requirements and gather feedback.

#### 2. **Team App and Home Page Customization**

- **Create New AM Team App**:
    - Use Salesforce App Manager to create a new Lightning app.
    - Add relevant tabs and components to the app.
- **Customize Home Page**:
    - Use Salesforce Lightning App Builder to create a custom home page for the AM Team.
    - Add required widgets:
        - Close deals
        - Plan My Accounts
        - Grow Relationships
        - Build Pipeline
        - Open tasks
        - Overdue tasks
        - Today’s events
        - Current quarter performance
        - Sales / NDR / Meetings attended

#### 3. **New Calendar Event Record Type**

- **Create New Record Types**:
    - Navigate to Object Manager > Event > Record Types.
    - Create record types for “Monthly Account Review,” “Quarterly Business Review,” and “Annual Business Review.”
- **Page Layouts**:
    - Customize page layouts for these record types as needed.
- **Create Event Form with Google Form Integration**:
    - Use Google Forms to create the required form.
    - Use Salesforce Flow or a third-party integration tool to automate the process of pre-filling and linking the Google Form to Salesforce events.

#### 4. **Event and Task Automation**

- **Automate Task Creation**:
    - Use Salesforce Process Builder or Flow to create rules for auto-generating tasks and events.
    - Monthly, Quarterly, and Annual meetings can be scheduled automatically at the beginning of the month.
- **Overdue Contact Flags**:
    - Use Salesforce workflows or Process Builder to create rules for overdue contact flags for accounts and opportunities.

#### 5. **Miro Account Management Cadence Integration**

- **Integrate Miro Cadence**:
    - Use the Miro integration tool for Salesforce to link the Miro account management cadence.
    - Follow Miro’s documentation to set up and configure the integration within Salesforce.

#### 6. **Access and Training for Sales Engage Cadences**

- **Training Sessions**:
    - Schedule training sessions for users to understand how to use Sales Engage Cadences.
    - Use Salesforce’s Trailhead modules and documentation for self-paced learning.
- **Documentation**:
    - Provide step-by-step guides and resources to help users.

#### 7. **Mail Merge Creation**

- **Mail Merge Tools**:
    - Explore Salesforce’s native mail merge functionality or consider third-party tools like Conga Composer.
    - Provide training and documentation on how to use the chosen tool.

#### 8. **Leverage Salesforce Einstein**

- **Einstein Benefits**:
    - Schedule a session with Salesforce experts or consultants to understand the benefits of Salesforce Einstein for leads, opportunities, and accounts.
    - Use Trailhead modules to learn about Einstein Analytics and its features.

#### 9. **Home Page Dashboard Creation**

- **Create Custom Dashboard**:
    - Use Salesforce Dashboard Builder to create a dashboard highlighting:
        - Performance MTD, QTD, and YTD for the team.
        - Pipeline closing in the month.
        - Accounts not contacted in 30 days.
        - Opportunities not contacted in 30 days.
        - WTD activity for the team.
        - Opportunities by stage.
        - Team sales by individual YTD.

### Action Plan and Timeline

#### Week 1:

- Kickoff meeting and requirement clarification.
- Create and configure the new AM Team app.
- Customize the home page with required widgets.

#### Week 2:

- Set up new calendar event record types.
- Develop and integrate the Google Form for event reports.
- Automate task and event creation processes.

#### Week 3:

- Integrate Miro account management cadence.
- Conduct training sessions for Sales Engage Cadences.
- Explore mail merge tools and provide training.

#### Week 4:

- Schedule Einstein benefits session and complete training.
- Build and finalize the custom dashboard.

### Additional Tips for the Meeting

- **Prepare Questions**: Write down any questions you have regarding unclear specifications or dependencies.
- **Highlight Non-Code Solutions**: Emphasize the use of Salesforce’s declarative tools like Flow, Process Builder, and App Builder.
- **Documentation**: Prepare to show any initial drafts or sketches of the home page and dashboard to gather feedback early.
- **Follow-Up Plan**: Outline a plan for follow-up meetings to track progress and make adjustments as necessary.