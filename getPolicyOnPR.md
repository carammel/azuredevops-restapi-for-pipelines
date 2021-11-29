# Check Policy on Pull Requests order by reviewer and status
## If you are looking for a way to check policy on Pull Requests you can use evaluations. I didn't know what are they until start searching. 
### Need:
 - Find out policy reqiurements are satisfied on PRs that is assigned to my group. 
### Process1:
 - Search PR docs on https://docs.microsoft.com/en-us/rest/api/azure/devops/git/pull-requests/get-pull-requests?view=azure-devops-rest-6.1
 - Using link below I could get the PRs
   - GET https://{instance}/{collection}/{project}/_apis/git/pullrequests?searchCriteria.includeLinks={searchCriteria.includeLinks}&searchCriteria.sourceRefName={searchCriteria.sourceRefName}&searchCriteria.sourceRepositoryId={searchCriteria.sourceRepositoryId}&searchCriteria.targetRefName={searchCriteria.targetRefName}&searchCriteria.status={searchCriteria.status}&searchCriteria.reviewerId={searchCriteria.reviewerId}&searchCriteria.creatorId={searchCriteria.creatorId}&searchCriteria.repositoryId={searchCriteria.repositoryId}&maxCommentLength={maxCommentLength}&$skip={$skip}&$top={$top}&api-version=6.1
   - GET https://{instance}/{collection}/{project}/_apis/git/pullrequests?searchCriteria.status=active&searchCriteria.reviewerId={searchCriteria.reviewerId}&api-version=6.1
   - Note: Find out the reviewerId a PR can be used to get it with the link below
     - https://devops/Dev_Collection/APIs/_apis/git/pullrequests/{pullRequestId}?api-versison=6.1

### Process2:
 - I could find policies that check PR's situation with below link:
   - GET https://dev.azure.com/{organization}/{project}/_apis/policy/evaluations?artifactId={artifactId}&api-version=6.1-preview.1 
 - I tried to find artifactId, and I realised that on result of get PR by PRId there is a artifactId but it is useless. 
   - My result on PR get result: "artifactId": "vstfs:///Git/PullRequestId/bb78930c-f728-424c-90c9-bd664763eecf%2fdc2c7cc3-22e3-4700-b3c4-d2545da56r62%2f22509" 
 - On doc, a template is wrote but at first sight this can be thought that we need to code for generation. 
   - This format should be artifactId: vstfs:///CodeReview/CodeReviewId/{projectId}/{pullRequestId} 
   - I put all variables to the link and voila :)
     -  https://{instance}/{collection}/{project}/_apis/policy/evaluations?artifactId=vstfs:///CodeReview/CodeReviewId/{projectId}/{pullRequestId}&api-version=6.0-preview.1
 
 ### Result:
  - I can get PR's that my team is assigned as reviewer and the policies are checked or not :)
    - check my powershell and python code to find it. 
      - getPRwithPolicy.ps1
      - getPRwithPolicy.py  
