# Fixing Assignment Timezone Issues in AI Context

## Planning
- [x] Investigate how assignment deadlines are formatted in the AI system prompt (`brief.py` or context builder).
- [x] Identify if the timezone conversion is missing (UTC vs Asia/Singapore).
- [x] Investigate the Canvas sync logic (`canvas_sync.py` or similar) to see if dates are saved in UTC or local time.
- [x] Create an implementation plan to ensure all assignment dates provided to the AI are in `Asia/Singapore` timezone.

## Execution
- [x] Create helper function `utc_to_sg` in `timezone.py`.
- [x] Apply timezone conversion to the assignment data context in `brief.py`.
- [x] Apply timezone conversion to the `search_assignments` AI tool.

## Verification
- [x] Reloaded backend instance natively to apply changes.
- [x] Asked the AI for assignment deadlines via UI browser test. Screenshots verify that 18:30 is listed instead of the erroneous 10:30 UTC.
