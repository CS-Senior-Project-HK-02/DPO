from datasets import load_dataset
from collections import defaultdict 
class DataInstruct:
    name = r"/home/hwalke37/dataset/data_instruct.json"

    def __init__(self, split: str = "train"):
        super().__init__()
        self.split = split
        dataset = load_dataset(self.name, split=self.split)
        self.dataset_dict = defaultdict(dict)
        for item in dataset:
            post_id = item["question"]["id"]
            if post_id not in self.dataset_dict.keys():
                self.dataset_dict[post_id] = {
                    "full_text": item["question"]["full_text"],
                    "answers": [],
                }
                if item["score_0"] > 0:
                    answers = [item["answer_0"], item["answer_1"]]
                elif item["score_0"] < 0:
                    answers = [item["answer_1"], item["answer_0"]]
                else:
                    answers = []
                answers = [re.sub(r"\[\d+\]", "", answer) for answer in answers]
                answers = [
                    ".".join([sent.strip() for sent in answer.split(".")])
                    for answer in answers
                ]
                if answers:
                    self.dataset_dict[post_id]["answers"].extend(answers)
                else:
                    _ = self.dataset_dict.pop(post_id)

        self.post_ids = list(self.dataset_dict.keys())

    def __len__(self):
        return len(self.post_ids)

    def __getitem__(self, idx):
        question, answers = self.dataset_dict[self.post_ids[idx]].values()
        return question, answers
