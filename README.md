# How to create a Spaced Study question bank
This repository serves as a guide for those wanting to create their own Spaced Study question bank for students to use.

If you have any questions or need help, feel free to [open an issue.](https://github.com/spacedstudy/create-question-bank/issues/new)

---

- Spaced Study uses `.spacedstudy` file format to store its question banks. This is effectively just a `.json.gz` file.
- To start, create a `.json` file. You can call it whatever you'd like. Later, you'll gzip this file to yield `.json.gz` and finally rename its extension to `.spacedstudy`
- You'll need to host a direct download link to this file somewhere. Popular solutions are GitHub's "raw" feature or Google Drive (but I wouldn't recommend gdrive as they tend to break solutions and are not really designed for [direct gdrive download links](https://sites.google.com/site/gdocs2direct/))

### Question Bank Basic Information

```json
{
  name: 'My Awesome Question Bank',
  description: "This question bank is awesome. Spaced Study is awesome.",
  ...
}
  ```

### Questions

Create a `questions` array in the json file.

Each question object within `questions` should look something like this:
```json
{
  category: 'Unit 1',
  prompt: '<p>Which choice most logically completes the text?</p>',
  desmos: false,
  stimulus: '<p>Cool</p>',
  answers: [
    '<p>Answer A</p>',
    '<p>Answer B</p>',
    '<p>Answer C</p>',
    '<p>Answer D</p>'
  ],
  correctIndex: 1,
  explanation: '<p>Choice B is the best answer because B stands for Best!!!</p>',
  embedding: [
      0.052849665, 0.016160311, 0.04307646, 0.039013144, 0.043448266, ...
  ]
}
```
If you don't want to include an explanation, make sure to set `explanation: null`. **Nothing else can be omitted.**

Keep in mind, you can use MathML for math expressions in the prompt, stimulus, or explanation, or answers. You can use `svg` or `img` with base64 for diagrams. Practically all *non-scripted* HTML can be included.

To generate `embedding`, I suggest you use OpenAI's `text-embedding-3-small`. However, please don't feel limited by this. There is no specific length that the embeddings need to be (as long as all of the embeddings in a question bank are of the same length, you'll be good). These embeddings are very important and are used to give targeted questions to the student.

### Question Bank Category Weighing
This is useful when you want to balance an imbalanced question set according to a set of weights. If a test is comprised with 20% of one unit and other weights for other units, then it makes sense to define it here.
```json
{
  ...
  categoryWeights: {
    'Unit 1': 20,
    'Unit 2': 20,
    ...
  },
  ...
}
```
If you don't want to use this feature, make sure to set `categoryWeights: null`

### Question Bank Timing Presets
Many tests have a certain amount of time allocated for each question. Spaced Study has a timer built-in so students can ensure they are answering questions at the appropriate pace.
```json
{
  ...
  timingPresets: [{
	    name: 'Normal Pacing',
	    default: true,
	    categoryToSeconds: {
	      'Unit 1': 35,
	      'Unit 2': 35,
	      ...
	    }
	  },
	  ...
  ],
  ...
}
```

If you want to disable the built-in timer for your question bank, make sure to set `timingPresets: []` You are strongly encouraged to make more than one timing preset for students wishing to go at a quicker pace so they can check their work at the end in a real testing situation, or students who have testing accommodations and need extra time.
