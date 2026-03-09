# Task: Enhancing AI Retrieval and Linking for Module Materials

- [x] Planning Phase <!-- id: 0 -->
    - [x] Research existing RAG and search implementations <!-- id: 1 -->
    - [x] Design implementation for improved retrieval and linking <!-- id: 2 -->
- [x] Implementation Phase <!-- id: 3 -->
    - [x] Update `ai_service.py` with better material retrieval logic <!-- id: 4 -->
    - [x] Add linking capabilities to AI responses (backend/frontend) <!-- id: 5 -->
    - [x] Implement/Update `search_module_materials` tool logic <!-- id: 6 -->
- [x] Verification Phase <!-- id: 7 -->
    - [x] Test retrieval with sample queries <!-- id: 8 -->
    - [x] Verify links are correctly formatted and functional in UI <!-- id: 9 -->
- [x] UI Link Rendering Fix <!-- id: 10 -->
    - [x] Update backend to use relative URIs instead of `file:///` <!-- id: 11 -->
    - [x] Configure `react-markdown` in frontend to allow and style these links <!-- id: 12 -->
- [x] Canvas Link Integration <!-- id: 13 -->
    - [x] Update `CanvasFile` and `ModuleFile` models to store Canvas URLs if available <!-- id: 14 -->
    - [x] Update AI tools to return Canvas URLs <!-- id: 15 -->
    - [x] Update frontend to show "Open in Canvas" button alongside download <!-- id: 16 -->
