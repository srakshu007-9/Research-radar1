# ResearchRadar – targeted changes

This update is based on the previous `ResearchRadar_Final_Integrated_FIXED` project. The existing frontend design and application structure have been retained.

## Changes

1. PDF Research Reader: asks the configured Groq model for a dedicated `research_summary` covering purpose, approach, results and conclusion, and displays it separately from the extracted abstract and structured sections. It continues to label the local extraction fallback as not being an AI-verified summary.
2. AI recommendations: uses a richer academic evaluation prompt over actual MySQL search results. Each ranked paper can include relevance score, detailed reason, topic alignment, methodology/focus, usefulness, limitations/missing information and reading priority. The response also includes a landscape synthesis and suggested research directions.
3. Paper cards: retain title, authors and abstract; show detailed recommendation reasoning where available; expose clearly labelled Save/Unsave, Details, Open source and Download PDF actions when a PDF URL exists.
4. Help Desk: signed-in users can submit categorized support tickets. Tickets are stored in MySQL. The configured project administrator can view the submitted ticket list in the Help Desk.

## Help Desk setup

Set `HELPDESK_ADMIN_EMAIL` in `backend/.env` to the exact email address of the ResearchRadar administrator account. Sign in using that account to see the submitted ticket list. Other signed-in users can submit tickets but cannot view other users' tickets.

The new `support_tickets` table is included in `backend/database/schema.sql`; the Help Desk endpoint also creates it on first use if the database user has CREATE TABLE permission.

## Notes

- The original AI recommendation/RAG module remains preserved under `source_modules/ai_recommendation_uploaded_original/`. Its original Qdrant, Gemini and Redis integrations require their own external credentials and are not silently substituted into the MySQL-backed application. The integrated recommendation endpoint now produces more detailed evidence-grounded recommendations from the live MySQL paper records.
- AI summaries and AI recommendations require a valid Groq API key and an available model in `backend/.env`.
- The browser PDF library remains local to the browser (IndexedDB); uploaded PDF files are not automatically sent to the project creator.
