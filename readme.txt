========================================================================
 EVALUATION TEST DATASETS  (for agent: KnowledgeTest)
========================================================================

WHICH FILE TO UPLOAD
--------------------
  invoice-tests-graded.jsonl   <-- USE THIS ONE for a real evaluation.
                                   Has query + ground_truth, so correctness
                                   evaluators actually have something to
                                   compare against.

  invoice-tests.jsonl          Queries only. Quick first pass; the service
                               runs your agent and scores the responses, but
                               without ground truth some scores can look
                               misleadingly high.

  invoice-tests.csv            Same idea as JSONL, in spreadsheet form
                               (turn-level only). "response" is left blank on
                               purpose -- for an Agent target, Foundry fills it
                               by running your agent on each query.

  sim-users.jsonl              Multi-turn "simulated user" scenarios. Use only
                               when you choose the "simulate conversations"
                               data source, not the plain dataset upload.


IMPORTANT: GROUND TRUTH MUST MATCH YOUR AGENT'S KNOWLEDGE
--------------------------------------------------------
The ground_truth answers here match the Northwind Traders Employee Handbook
(the sample PDF you uploaded to your knowledge base). If your KnowledgeTest
agent is grounded on a DIFFERENT document, edit the ground_truth values so they
match that document -- otherwise correct answers will be scored as wrong.


IN THE CREATE EVALUATION WIZARD
-------------------------------
  1. Target type: Agent  ->  select KnowledgeTest
  2. Data source: Existing dataset -> Upload new dataset
                  name it "invoice-tests-graded", upload the graded .jsonl
  3. Field mapping:
        Query        -> {{item.query}}
        Ground truth -> {{item.ground_truth}}
        Response     -> leave as the agent-generated response
  4. Evaluators: Intent Resolution, Task Adherence, Tool Call Accuracy,
                 Groundedness, Relevance, Coherence (+ safety if you want).
                 Pick a GPT judge model (e.g. gpt-4o-mini) when asked.
  5. Name the run and Submit. Results appear under Evaluation in a few minutes.


VALIDATE BEFORE UPLOADING (optional)
------------------------------------
  python -c "import json;[json.loads(l) for l in open('invoice-tests-graded.jsonl')]"
  (No output means the file is valid JSONL.)


NOTE ON THE LAST ROW
--------------------
The "pet-adoption policy" case is an out-of-scope test on purpose. A well-
grounded agent should say it does not know instead of inventing an answer --
a good check for task adherence and groundedness.
========================================================================
