# 1. Identify Learning Objectives from Student's Working

## 1.1. Goal of this project

This project is a proof-of-concept for possible advancement in the capabilites of the product that I am working on.

The product is a online platform which can let students attempt Maths questions with full workings, and the system will automatically mark every step as correct or wrong.

Currently, the system can mark a working to a question as either fully correct or highlight the mistakes to the students.

However, if a working has been marked as fully correct, the system can not detect if the student had achieved the learnings objectives that had been set in the question.

Every question has been authored with a **set of learning objectives** and the author had provided **a full working that achieved all the learning objectives**.

The learning objectives used in the product are derived from MOE's learning experience for Secondary School Mathematics.

* https://www.moe.gov.sg/docs/default-source/document/education/syllabuses/sciences/files/mathematics-syllabus-sec-1-to-4-express-n(a)-course.pdf#page=34

The goal of this project is to indentify the learning objectives that has been achieved in a working, hence when the student enter his working, the model can be used to predict the learning objectives achieved by him, and match against the learning objectives that was set for the question by the author.

## 1.2. Conclusion

* **> 80%** accuracy on predicting chapter
* **> 50%** accuracy on predicting a single learning objective

This demostrates that it is feasible to encode a Math expression so that a model can use it as input and achieve meaningful prediction.

It is especially promising because the data used in this project is only one type of data that are assoicated to every questions. There other data in the system that can be associated to the questions and workings which can be used to enchance these models further.

Further work will be done to explore using machine learning to advance the capaibilities of the product.

## 1.3. Further Work

1. Try a All-You-Need-Is-Attention model on the same input and output data.
2. Try to encode the input data into a 2D picture and try to use a CNN model to train on it.
3. Try to build a model to detect the types of equations in a working.
    * While predicting the learning objectives achieved in a whole working might not be feasible,
    * it might still be possible to predict the type of equations on the individual equations in the whole working.
    * Then another model can use the predicted types of equations,
    * to further predict the learning objectives achieved.
    * However, presently, we do not have enough labelled equations to train on this approach.