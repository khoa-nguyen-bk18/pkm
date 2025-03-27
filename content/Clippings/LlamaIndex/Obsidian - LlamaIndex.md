---
title: "Obsidian - LlamaIndex"
source: "https://docs.llamaindex.ai/en/stable/api_reference/readers/obsidian/"
author:
published:
created: 2025-03-15
description:
tags:
  - "clippings"
---
## Obsidian

## ObsidianReader [#](https://docs.llamaindex.ai/en/stable/api_reference/readers/obsidian/#llama_index.readers.obsidian.ObsidianReader "Permanent link")

Bases: `[BaseReader](https://docs.llamaindex.ai/en/stable/api_reference/readers/#llama_index.core.readers.base.BaseReader "llama_index.core.readers.base.BaseReader")`

input\_dir (str): Path to the Obsidian vault. extract\_tasks (bool): If True, extract tasks from the text and store them in metadata. Default is False. remove\_tasks\_from\_text (bool): If True and extract\_tasks is True, remove the task lines from the main document text. Default is False.

Source code in `llama-index-integrations/readers/llama-index-readers-obsidian/llama_index/readers/obsidian/base.py`
```
32
 33
 34
 35
 36
 37
 38
 39
 40
 41
 42
 43
 44
 45
 46
 47
 48
 49
 50
 51
 52
 53
 54
 55
 56
 57
 58
 59
 60
 61
 62
 63
 64
 65
 66
 67
 68
 69
 70
 71
 72
 73
 74
 75
 76
 77
 78
 79
 80
 81
 82
 83
 84
 85
 86
 87
 88
 89
 90
 91
 92
 93
 94
 95
 96
 97
 98
 99
100
101
102
103
104
105
106
107
108
109
110
111
112
113
114
115
116
117
118
119
120
121
122
123
124
125
126
127
128
129
130
131
132
133
134
135
136
137
138
139
140
141
142
143
144
145
146class ObsidianReader(BaseReader):
    """
    input_dir (str): Path to the Obsidian vault.
    extract_tasks (bool): If True, extract tasks from the text and store them in metadata.
                            Default is False.
    remove_tasks_from_text (bool): If True and extract_tasks is True, remove the task
                                    lines from the main document text.
                                    Default is False.
    """

    def __init__(
        self,
        input_dir: str,
        extract_tasks: bool = False,
        remove_tasks_from_text: bool = False,
    ):
        self.input_dir = Path(input_dir)
        self.extract_tasks = extract_tasks
        self.remove_tasks_from_text = remove_tasks_from_text

    def load_data(self, *args: Any, **load_kwargs: Any) -> List[Document]:
        """
        Walks through the vault, loads each markdown file, and adds extra metadata
        (file name, folder path, wikilinks, backlinks, and optionally tasks).
        """
        docs: List[Document] = []
        # This map will hold: {target_note: [linking_note1, linking_note2, ...]}
        backlinks_map = {}

        for dirpath, dirnames, filenames in os.walk(self.input_dir):
            # Skip hidden directories.
            dirnames[:] = [d for d in dirnames if not d.startswith(".")]
            for filename in filenames:
                if filename.endswith(".md"):
                    filepath = os.path.join(dirpath, filename)
                    md_docs = MarkdownReader().load_data(Path(filepath))
                    for i, doc in enumerate(md_docs):
                        file_path_obj = Path(filepath)
                        note_name = file_path_obj.stem
                        doc.metadata["file_name"] = file_path_obj.name
                        doc.metadata["folder_path"] = str(file_path_obj.parent)
                        doc.metadata["folder_name"] = str(
                            file_path_obj.parent.relative_to(self.input_dir)
                        )
                        doc.metadata["note_name"] = note_name
                        wikilinks = self._extract_wikilinks(doc.text)
                        doc.metadata["wikilinks"] = wikilinks
                        # For each wikilink found in this document, record a backlink from this note.
                        for link in wikilinks:
                            # Each link is expected to match a note name (without .md)
                            backlinks_map.setdefault(link, []).append(note_name)

                        # Optionally, extract tasks from the text.
                        if self.extract_tasks:
                            tasks, cleaned_text = self._extract_tasks(doc.text)
                            doc.metadata["tasks"] = tasks
                            if self.remove_tasks_from_text:
                                md_docs[i] = Document(
                                    text=cleaned_text, metadata=doc.metadata
                                )
                    docs.extend(md_docs)

        # Now that we have processed all files, assign backlinks metadata.
        for doc in docs:
            note_name = doc.metadata.get("note_name")
            # If no backlinks exist for this note, default to an empty list.
            doc.metadata["backlinks"] = backlinks_map.get(note_name, [])
        return docs

    def load_langchain_documents(self, **load_kwargs: Any) -> List["LCDocument"]:
        """
        Loads data in the LangChain document format.
        """
        docs = self.load_data(**load_kwargs)
        return [d.to_langchain_format() for d in docs]

    def _extract_wikilinks(self, text: str) -> List[str]:
        """
        Extracts Obsidian wikilinks from the given text.

        Matches patterns like:
          - [[Note Name]]
          - [[Note Name|Alias]]

        Returns a list of unique wikilink targets (aliases are ignored).
        """
        pattern = r"\[\[([^\]]+)\]\]"
        matches = re.findall(pattern, text)
        links = []
        for match in matches:
            # If a pipe is present (e.g. [[Note|Alias]]), take only the part before it.
            target = match.split("|")[0].strip()
            links.append(target)
        return list(set(links))

    def _extract_tasks(self, text: str) -> Tuple[List[str], str]:
        """
        Extracts markdown tasks from the text.

        A task is expected to be a checklist item in markdown, for example:
            - [ ] Do something
            - [x] Completed task

        Returns a tuple:
            (list of task strings, text with task lines removed if removal is enabled).
        """
        # This regex matches lines starting with '-' or '*' followed by a checkbox.
        task_pattern = re.compile(
            r"^\s*[-*]\s*\[\s*(?:x|X| )\s*\]\s*(.*)$", re.MULTILINE
        )
        tasks = task_pattern.findall(text)
        cleaned_text = text
        if self.remove_tasks_from_text:
            cleaned_text = task_pattern.sub("", text)
        return tasks, cleaned_text
```

### load\_data [#](https://docs.llamaindex.ai/en/stable/api_reference/readers/obsidian/#llama_index.readers.obsidian.ObsidianReader.load_data "Permanent link")

```
load_data(*args: Any, **load_kwargs: Any) -> List[Document]
```

Walks through the vault, loads each markdown file, and adds extra metadata (file name, folder path, wikilinks, backlinks, and optionally tasks).

Source code in `llama-index-integrations/readers/llama-index-readers-obsidian/llama_index/readers/obsidian/base.py`
```
52
53
54
55
56
57
58
59
60
61
62
63
64
65
66
67
68
69
70
71
72
73
74
75
76
77
78
79
80
81
82
83
84
85
86
87
88
89
90
91
92
93
94
95
96
97
98
99def load_data(self, *args: Any, **load_kwargs: Any) -> List[Document]:
    """
    Walks through the vault, loads each markdown file, and adds extra metadata
    (file name, folder path, wikilinks, backlinks, and optionally tasks).
    """
    docs: List[Document] = []
    # This map will hold: {target_note: [linking_note1, linking_note2, ...]}
    backlinks_map = {}

    for dirpath, dirnames, filenames in os.walk(self.input_dir):
        # Skip hidden directories.
        dirnames[:] = [d for d in dirnames if not d.startswith(".")]
        for filename in filenames:
            if filename.endswith(".md"):
                filepath = os.path.join(dirpath, filename)
                md_docs = MarkdownReader().load_data(Path(filepath))
                for i, doc in enumerate(md_docs):
                    file_path_obj = Path(filepath)
                    note_name = file_path_obj.stem
                    doc.metadata["file_name"] = file_path_obj.name
                    doc.metadata["folder_path"] = str(file_path_obj.parent)
                    doc.metadata["folder_name"] = str(
                        file_path_obj.parent.relative_to(self.input_dir)
                    )
                    doc.metadata["note_name"] = note_name
                    wikilinks = self._extract_wikilinks(doc.text)
                    doc.metadata["wikilinks"] = wikilinks
                    # For each wikilink found in this document, record a backlink from this note.
                    for link in wikilinks:
                        # Each link is expected to match a note name (without .md)
                        backlinks_map.setdefault(link, []).append(note_name)

                    # Optionally, extract tasks from the text.
                    if self.extract_tasks:
                        tasks, cleaned_text = self._extract_tasks(doc.text)
                        doc.metadata["tasks"] = tasks
                        if self.remove_tasks_from_text:
                            md_docs[i] = Document(
                                text=cleaned_text, metadata=doc.metadata
                            )
                docs.extend(md_docs)

    # Now that we have processed all files, assign backlinks metadata.
    for doc in docs:
        note_name = doc.metadata.get("note_name")
        # If no backlinks exist for this note, default to an empty list.
        doc.metadata["backlinks"] = backlinks_map.get(note_name, [])
    return docs
```

### load\_langchain\_documents [#](https://docs.llamaindex.ai/en/stable/api_reference/readers/obsidian/#llama_index.readers.obsidian.ObsidianReader.load_langchain_documents "Permanent link")

```
load_langchain_documents(**load_kwargs: Any) -> List[Document]
```

Loads data in the LangChain document format.

Source code in `llama-index-integrations/readers/llama-index-readers-obsidian/llama_index/readers/obsidian/base.py`
```
101
102
103
104
105
106def load_langchain_documents(self, **load_kwargs: Any) -> List["LCDocument"]:
    """
    Loads data in the LangChain document format.
    """
    docs = self.load_data(**load_kwargs)
    return [d.to_langchain_format() for d in docs]
```