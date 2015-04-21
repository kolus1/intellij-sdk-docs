---
layout: editable
title: General Threading Rules
---

In general, the data structures of IntelliJ IDEA are covered by a single "multiple readers / single writer" lock.
Reading data is allowed from any thread.
Reading data from the UI thread does not require any special effort, however, read operations performed from any other thread need to be wrapped in a read action by using ```ApplicationManager.getApplication().runReadAction()```.
Writing the data is only allowed from the UI thread, and write operations always need to be wrapped in a write action with ```ApplicationManager.getApplication().runWriteAction()```.

To pass control from a background thread to the event dispatch thread, instead of the standard ```SwingUtilities.invokeLater()```, plugins should use ```ApplicationManager.getApplication().invokeLater()```.
The latter API allows to specify the _modality state_ for the call - the stack of modal dialogs under which the call is allowed to execute.
Passing ```ModalityState.NON_MODAL``` means that the operation will be executed after all modal dialogs are closed, and passing ```ModalityState.stateForComponent()``` means that the operation may be executed while the specified component (part of a dialog) is still visible.