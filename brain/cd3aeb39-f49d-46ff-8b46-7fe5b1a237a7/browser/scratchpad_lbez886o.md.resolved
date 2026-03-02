# AI Chat Testing Plan

1.  **Open URL**: Navigate to `http://localhost:3000`. (Done)
2.  **Wait for Load**: Give the page 3-5 seconds to load. (Done)
3.  **Find Chat Input**: Locate the textarea in the "Command Center" panel. (Done)
4.  **Send Query**:
    *   Click the textarea.
    *   Type: "What are the key topics covered in the BT1101 probability distributions lecture?".
    *   Click the send button. (Done)
5.  **Observe Response**: Wait for 15-20 seconds and verify tool usage in the response. (Done - AI responded but tool usage failed/errored)
6.  **Capture Response**: Take a screenshot of the response. (Done: `ai_chat_response_failure_1772481112705.png`)
7.  **Test "CLEAR" Button**: Click the "CLEAR" text near the prompt recipe chips. (Done)
8.  **Verify Clear**: Wait 2 seconds and take a screenshot to confirm the clear worked. (Done: `ai_chat_cleared_1772481147788.png`)
9.  **Report Findings**:
    *   AI responded: Yes.
    *   Successful material retrieval: No (AI apologized and said it couldn't access content).
    *   Clear button worked: Yes (Confirmed by POST 200 and visual state).
    *   Errors: 401s on some background endpoints (timetable, etc.), but Clear worked fine.
