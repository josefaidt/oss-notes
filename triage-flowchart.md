# GitHub Issue Triage Flowchart

In the flowchart below, GitHub issues will be triaged into one of three issue types:

- `bug`
- `feature-request`
- `question`

```mermaid
flowchart TD
    issueCreated[issue is created from:]
    fromTemplateBug[bug template]
    fromTemplateFeatureRequest[feature-request template]
    fromTemplateQuestion[question template]
    fromTemplateNone[existing comment or `/issues/new`]
    fromTransferred[staff transfers issue from another repository]

    issueCreated-->fromTemplateBug
    issueCreated-->fromTemplateFeatureRequest
    issueCreated-->fromTemplateQuestion
    issueCreated-->fromTemplateNone
    issueCreated-->fromTransferred

    fromTemplateBug-->|pending-triage label added|entityGitHubIssueList
    fromTemplateFeatureRequest-->|pending-triage label added|entityGitHubIssueList
    fromTemplateQuestion-->|pending-triage label added|entityGitHubIssueList
    fromTemplateNone-->|no label added|entityGitHubIssueList
    fromTransferred-->|staff manually adds `transferred` label|entityGitHubIssueList

    entityGitHubIssueList(GitHub issue list)
    entityTriageQueue(triage queue)

    subgraph triage
        triageBegins[staff evaluates, applies category labels as necessary]

        %% staff response flow
        triageStaffResponse[staff responds]
        triageStaffAddsPendingResponse[staff adds pending-response label]
        triageAuthorResponse[author responds]
        triageGitHubActionsRemovesPendingResponse[GitHub Actions removes pending-response label]

        triageBegins-->triageStaffResponse
        triageStaffResponse-->triageStaffAddsPendingResponse
        triageStaffAddsPendingResponse-->triageAuthorResponse
        triageAuthorResponse-->triageGitHubActionsRemovesPendingResponse

        %% staff reproduction flow
        triageStaffConfirmsBug[staff confirms bug]
        triageStaffConfirmsFeatureRequest[staff confirms feature-request]
        triageStaffAnswersQuestion[staff answers question]

        triageBegins-->|staff is able to reproduce|triageStaffConfirmsBug
        triageBegins-->triageStaffConfirmsFeatureRequest
        triageBegins-->triageStaffAnswersQuestion

        %% triageStaffLabelCorrection[staff corrects labels as needed]
    end

    triageGitHubActionsRemovesPendingResponse-->entityTriageQueue
    entityGitHubIssueList-->entityTriageQueue
    entityTriageQueue-->triageBegins

    entityQueuePrioritization(prioritization queue)
    entityQueueBug(bug queue)
    entityQueueFeatureRequest(feature-request queue)

    triageStaffAnswersQuestion-->issueStateClosed
    triageStaffConfirmsBug-->|staff adds bug label|entityQueuePrioritization
    triageStaffConfirmsFeatureRequest-->|staff adds feature-request label|entityQueuePrioritization

    entityQueuePrioritization-->|staff adds p0-p3 label|entityQueueBug
    entityQueuePrioritization-->|staff adds p3-p4 label|entityQueueFeatureRequest
    entityRelease(product is released)
    entityQueueBug-->|bug is fixed|entityRelease
    entityQueueFeatureRequest-->|feature is implemented|entityRelease

    issueStateClosed[issue is closed]
    entityRelease-->issueStateClosed
```
