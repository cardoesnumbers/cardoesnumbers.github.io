---
id: 20
title: 'Opioid Usage in the US (2006-2012)'
date: '2023-07-16T12:04:45+00:00'
author: urtechoc
layout: post
guid: 'https://cardoesnumbers.se/?p=20'
permalink: /2023/07/16/pain-management-research-in-progress/
image: 'http://cardoesnumbers.se/wp-content/uploads/2023/07/pexels-anna-shvets-3683047-scaled.jpg'
categories:
    - EDA
    - 'Jupyter Notebook'
---

The dataset I am going to be working with here is the [US pain pills set](https://www.kaggle.com/datasets/paultimothymooney/pain-pills-in-the-usa) from [Kaggle](https://www.kaggle.com/datasets/paultimothymooney/pain-pills-in-the-usa).

The unzipped set is massive (+75GB, tsv) so I will be using [dask](https://www.dask.org/) to handle it.

**Aug 2024 Update:**

I am picking up on this one after one year.

The problem I faced initially was not understading the .compute() command in Dask. I reviewed the work Paul Mooney did on its Kaggle notebook and found a work around it.

The result below.

<meta charset="utf-8"></meta><meta content="width=device-width, initial-scale=1.0" name="viewport"></meta><title>second\_pills</title><script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.10/require.min.js"></script><style type="text/css">
    pre { line-height: 125%; }
td.linenos .normal { color: inherit; background-color: transparent; padding-left: 5px; padding-right: 5px; }
span.linenos { color: inherit; background-color: transparent; padding-left: 5px; padding-right: 5px; }
td.linenos .special { color: #000000; background-color: #ffffc0; padding-left: 5px; padding-right: 5px; }
span.linenos.special { color: #000000; background-color: #ffffc0; padding-left: 5px; padding-right: 5px; }
.highlight .hll { background-color: var(--jp-cell-editor-active-background) }
.highlight { background: var(--jp-cell-editor-background); color: var(--jp-mirror-editor-variable-color) }
.highlight .c { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment */
.highlight .err { color: var(--jp-mirror-editor-error-color) } /* Error */
.highlight .k { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword */
.highlight .o { color: var(--jp-mirror-editor-operator-color); font-weight: bold } /* Operator */
.highlight .p { color: var(--jp-mirror-editor-punctuation-color) } /* Punctuation */
.highlight .ch { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.Hashbang */
.highlight .cm { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.Multiline */
.highlight .cp { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.Preproc */
.highlight .cpf { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.PreprocFile */
.highlight .c1 { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.Single */
.highlight .cs { color: var(--jp-mirror-editor-comment-color); font-style: italic } /* Comment.Special */
.highlight .kc { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Constant */
.highlight .kd { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Declaration */
.highlight .kn { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Namespace */
.highlight .kp { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Pseudo */
.highlight .kr { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Reserved */
.highlight .kt { color: var(--jp-mirror-editor-keyword-color); font-weight: bold } /* Keyword.Type */
.highlight .m { color: var(--jp-mirror-editor-number-color) } /* Literal.Number */
.highlight .s { color: var(--jp-mirror-editor-string-color) } /* Literal.String */
.highlight .ow { color: var(--jp-mirror-editor-operator-color); font-weight: bold } /* Operator.Word */
.highlight .pm { color: var(--jp-mirror-editor-punctuation-color) } /* Punctuation.Marker */
.highlight .w { color: var(--jp-mirror-editor-variable-color) } /* Text.Whitespace */
.highlight .mb { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Bin */
.highlight .mf { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Float */
.highlight .mh { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Hex */
.highlight .mi { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Integer */
.highlight .mo { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Oct */
.highlight .sa { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Affix */
.highlight .sb { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Backtick */
.highlight .sc { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Char */
.highlight .dl { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Delimiter */
.highlight .sd { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Doc */
.highlight .s2 { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Double */
.highlight .se { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Escape */
.highlight .sh { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Heredoc */
.highlight .si { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Interpol */
.highlight .sx { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Other */
.highlight .sr { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Regex */
.highlight .s1 { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Single */
.highlight .ss { color: var(--jp-mirror-editor-string-color) } /* Literal.String.Symbol */
.highlight .il { color: var(--jp-mirror-editor-number-color) } /* Literal.Number.Integer.Long */
  </style><style type="text/css">
/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*
 * Mozilla scrollbar styling
 */

/* use standard opaque scrollbars for most nodes */
[data-jp-theme-scrollbars='true'] {
  scrollbar-color: rgb(var(--jp-scrollbar-thumb-color))
    var(--jp-scrollbar-background-color);
}

/* for code nodes, use a transparent style of scrollbar. These selectors
 * will match lower in the tree, and so will override the above */
[data-jp-theme-scrollbars='true'] .CodeMirror-hscrollbar,
[data-jp-theme-scrollbars='true'] .CodeMirror-vscrollbar {
  scrollbar-color: rgba(var(--jp-scrollbar-thumb-color), 0.5) transparent;
}

/* tiny scrollbar */

.jp-scrollbar-tiny {
  scrollbar-color: rgba(var(--jp-scrollbar-thumb-color), 0.5) transparent;
  scrollbar-width: thin;
}

/*
 * Webkit scrollbar styling
 */

/* use standard opaque scrollbars for most nodes */

[data-jp-theme-scrollbars='true'] ::-webkit-scrollbar,
[data-jp-theme-scrollbars='true'] ::-webkit-scrollbar-corner {
  background: var(--jp-scrollbar-background-color);
}

[data-jp-theme-scrollbars='true'] ::-webkit-scrollbar-thumb {
  background: rgb(var(--jp-scrollbar-thumb-color));
  border: var(--jp-scrollbar-thumb-margin) solid transparent;
  background-clip: content-box;
  border-radius: var(--jp-scrollbar-thumb-radius);
}

[data-jp-theme-scrollbars='true'] ::-webkit-scrollbar-track:horizontal {
  border-left: var(--jp-scrollbar-endpad) solid
    var(--jp-scrollbar-background-color);
  border-right: var(--jp-scrollbar-endpad) solid
    var(--jp-scrollbar-background-color);
}

[data-jp-theme-scrollbars='true'] ::-webkit-scrollbar-track:vertical {
  border-top: var(--jp-scrollbar-endpad) solid
    var(--jp-scrollbar-background-color);
  border-bottom: var(--jp-scrollbar-endpad) solid
    var(--jp-scrollbar-background-color);
}

/* for code nodes, use a transparent style of scrollbar */

[data-jp-theme-scrollbars='true'] .CodeMirror-hscrollbar::-webkit-scrollbar,
[data-jp-theme-scrollbars='true'] .CodeMirror-vscrollbar::-webkit-scrollbar,
[data-jp-theme-scrollbars='true']
  .CodeMirror-hscrollbar::-webkit-scrollbar-corner,
[data-jp-theme-scrollbars='true']
  .CodeMirror-vscrollbar::-webkit-scrollbar-corner {
  background-color: transparent;
}

[data-jp-theme-scrollbars='true']
  .CodeMirror-hscrollbar::-webkit-scrollbar-thumb,
[data-jp-theme-scrollbars='true']
  .CodeMirror-vscrollbar::-webkit-scrollbar-thumb {
  background: rgba(var(--jp-scrollbar-thumb-color), 0.5);
  border: var(--jp-scrollbar-thumb-margin) solid transparent;
  background-clip: content-box;
  border-radius: var(--jp-scrollbar-thumb-radius);
}

[data-jp-theme-scrollbars='true']
  .CodeMirror-hscrollbar::-webkit-scrollbar-track:horizontal {
  border-left: var(--jp-scrollbar-endpad) solid transparent;
  border-right: var(--jp-scrollbar-endpad) solid transparent;
}

[data-jp-theme-scrollbars='true']
  .CodeMirror-vscrollbar::-webkit-scrollbar-track:vertical {
  border-top: var(--jp-scrollbar-endpad) solid transparent;
  border-bottom: var(--jp-scrollbar-endpad) solid transparent;
}

/* tiny scrollbar */

.jp-scrollbar-tiny::-webkit-scrollbar,
.jp-scrollbar-tiny::-webkit-scrollbar-corner {
  background-color: transparent;
  height: 4px;
  width: 4px;
}

.jp-scrollbar-tiny::-webkit-scrollbar-thumb {
  background: rgba(var(--jp-scrollbar-thumb-color), 0.5);
}

.jp-scrollbar-tiny::-webkit-scrollbar-track:horizontal {
  border-left: 0px solid transparent;
  border-right: 0px solid transparent;
}

.jp-scrollbar-tiny::-webkit-scrollbar-track:vertical {
  border-top: 0px solid transparent;
  border-bottom: 0px solid transparent;
}

/*
 * Phosphor
 */

.lm-ScrollBar[data-orientation='horizontal'] {
  min-height: 16px;
  max-height: 16px;
  min-width: 45px;
  border-top: 1px solid #a0a0a0;
}

.lm-ScrollBar[data-orientation='vertical'] {
  min-width: 16px;
  max-width: 16px;
  min-height: 45px;
  border-left: 1px solid #a0a0a0;
}

.lm-ScrollBar-button {
  background-color: #f0f0f0;
  background-position: center center;
  min-height: 15px;
  max-height: 15px;
  min-width: 15px;
  max-width: 15px;
}

.lm-ScrollBar-button:hover {
  background-color: #dadada;
}

.lm-ScrollBar-button.lm-mod-active {
  background-color: #cdcdcd;
}

.lm-ScrollBar-track {
  background: #f0f0f0;
}

.lm-ScrollBar-thumb {
  background: #cdcdcd;
}

.lm-ScrollBar-thumb:hover {
  background: #bababa;
}

.lm-ScrollBar-thumb.lm-mod-active {
  background: #a0a0a0;
}

.lm-ScrollBar[data-orientation='horizontal'] .lm-ScrollBar-thumb {
  height: 100%;
  min-width: 15px;
  border-left: 1px solid #a0a0a0;
  border-right: 1px solid #a0a0a0;
}

.lm-ScrollBar[data-orientation='vertical'] .lm-ScrollBar-thumb {
  width: 100%;
  min-height: 15px;
  border-top: 1px solid #a0a0a0;
  border-bottom: 1px solid #a0a0a0;
}

.lm-ScrollBar[data-orientation='horizontal']
  .lm-ScrollBar-button[data-action='decrement'] {
  background-image: var(--jp-icon-caret-left);
  background-size: 17px;
}

.lm-ScrollBar[data-orientation='horizontal']
  .lm-ScrollBar-button[data-action='increment'] {
  background-image: var(--jp-icon-caret-right);
  background-size: 17px;
}

.lm-ScrollBar[data-orientation='vertical']
  .lm-ScrollBar-button[data-action='decrement'] {
  background-image: var(--jp-icon-caret-up);
  background-size: 17px;
}

.lm-ScrollBar[data-orientation='vertical']
  .lm-ScrollBar-button[data-action='increment'] {
  background-image: var(--jp-icon-caret-down);
  background-size: 17px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-Widget, /*  */
.lm-Widget {
  box-sizing: border-box;
  position: relative;
  overflow: hidden;
  cursor: default;
}


/* <DEPRECATED> */ .p-Widget.p-mod-hidden, /*  */
.lm-Widget.lm-mod-hidden {
  display: none !important;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-CommandPalette, /*  */
.lm-CommandPalette {
  display: flex;
  flex-direction: column;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


/* <DEPRECATED> */ .p-CommandPalette-search, /*  */
.lm-CommandPalette-search {
  flex: 0 0 auto;
}


/* <DEPRECATED> */ .p-CommandPalette-content, /*  */
.lm-CommandPalette-content {
  flex: 1 1 auto;
  margin: 0;
  padding: 0;
  min-height: 0;
  overflow: auto;
  list-style-type: none;
}


/* <DEPRECATED> */ .p-CommandPalette-header, /*  */
.lm-CommandPalette-header {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}


/* <DEPRECATED> */ .p-CommandPalette-item, /*  */
.lm-CommandPalette-item {
  display: flex;
  flex-direction: row;
}


/* <DEPRECATED> */ .p-CommandPalette-itemIcon, /*  */
.lm-CommandPalette-itemIcon {
  flex: 0 0 auto;
}


/* <DEPRECATED> */ .p-CommandPalette-itemContent, /*  */
.lm-CommandPalette-itemContent {
  flex: 1 1 auto;
  overflow: hidden;
}


/* <DEPRECATED> */ .p-CommandPalette-itemShortcut, /*  */
.lm-CommandPalette-itemShortcut {
  flex: 0 0 auto;
}


/* <DEPRECATED> */ .p-CommandPalette-itemLabel, /*  */
.lm-CommandPalette-itemLabel {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.lm-close-icon {
	border:1px solid transparent;
  background-color: transparent;
  position: absolute;
	z-index:1;
	right:3%;
	top: 0;
	bottom: 0;
	margin: auto;
	padding: 7px 0;
	display: none;
	vertical-align: middle;
  outline: 0;
  cursor: pointer;
}
.lm-close-icon:after {
	content: "X";
	display: block;
	width: 15px;
	height: 15px;
	text-align: center;
	color:#000;
	font-weight: normal;
	font-size: 12px;
	cursor: pointer;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-DockPanel, /*  */
.lm-DockPanel {
  z-index: 0;
}


/* <DEPRECATED> */ .p-DockPanel-widget, /*  */
.lm-DockPanel-widget {
  z-index: 0;
}


/* <DEPRECATED> */ .p-DockPanel-tabBar, /*  */
.lm-DockPanel-tabBar {
  z-index: 1;
}


/* <DEPRECATED> */ .p-DockPanel-handle, /*  */
.lm-DockPanel-handle {
  z-index: 2;
}


/* <DEPRECATED> */ .p-DockPanel-handle.p-mod-hidden, /*  */
.lm-DockPanel-handle.lm-mod-hidden {
  display: none !important;
}


/* <DEPRECATED> */ .p-DockPanel-handle:after, /*  */
.lm-DockPanel-handle:after {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  content: '';
}


/* <DEPRECATED> */
.p-DockPanel-handle[data-orientation='horizontal'],
/*  */
.lm-DockPanel-handle[data-orientation='horizontal'] {
  cursor: ew-resize;
}


/* <DEPRECATED> */
.p-DockPanel-handle[data-orientation='vertical'],
/*  */
.lm-DockPanel-handle[data-orientation='vertical'] {
  cursor: ns-resize;
}


/* <DEPRECATED> */
.p-DockPanel-handle[data-orientation='horizontal']:after,
/*  */
.lm-DockPanel-handle[data-orientation='horizontal']:after {
  left: 50%;
  min-width: 8px;
  transform: translateX(-50%);
}


/* <DEPRECATED> */
.p-DockPanel-handle[data-orientation='vertical']:after,
/*  */
.lm-DockPanel-handle[data-orientation='vertical']:after {
  top: 50%;
  min-height: 8px;
  transform: translateY(-50%);
}


/* <DEPRECATED> */ .p-DockPanel-overlay, /*  */
.lm-DockPanel-overlay {
  z-index: 3;
  box-sizing: border-box;
  pointer-events: none;
}


/* <DEPRECATED> */ .p-DockPanel-overlay.p-mod-hidden, /*  */
.lm-DockPanel-overlay.lm-mod-hidden {
  display: none !important;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-Menu, /*  */
.lm-Menu {
  z-index: 10000;
  position: absolute;
  white-space: nowrap;
  overflow-x: hidden;
  overflow-y: auto;
  outline: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


/* <DEPRECATED> */ .p-Menu-content, /*  */
.lm-Menu-content {
  margin: 0;
  padding: 0;
  display: table;
  list-style-type: none;
}


/* <DEPRECATED> */ .p-Menu-item, /*  */
.lm-Menu-item {
  display: table-row;
}


/* <DEPRECATED> */
.p-Menu-item.p-mod-hidden,
.p-Menu-item.p-mod-collapsed,
/*  */
.lm-Menu-item.lm-mod-hidden,
.lm-Menu-item.lm-mod-collapsed {
  display: none !important;
}


/* <DEPRECATED> */
.p-Menu-itemIcon,
.p-Menu-itemSubmenuIcon,
/*  */
.lm-Menu-itemIcon,
.lm-Menu-itemSubmenuIcon {
  display: table-cell;
  text-align: center;
}


/* <DEPRECATED> */ .p-Menu-itemLabel, /*  */
.lm-Menu-itemLabel {
  display: table-cell;
  text-align: left;
}


/* <DEPRECATED> */ .p-Menu-itemShortcut, /*  */
.lm-Menu-itemShortcut {
  display: table-cell;
  text-align: right;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-MenuBar, /*  */
.lm-MenuBar {
  outline: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


/* <DEPRECATED> */ .p-MenuBar-content, /*  */
.lm-MenuBar-content {
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: row;
  list-style-type: none;
}


/* <DEPRECATED> */ .p--MenuBar-item, /*  */
.lm-MenuBar-item {
  box-sizing: border-box;
}


/* <DEPRECATED> */
.p-MenuBar-itemIcon,
.p-MenuBar-itemLabel,
/*  */
.lm-MenuBar-itemIcon,
.lm-MenuBar-itemLabel {
  display: inline-block;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-ScrollBar, /*  */
.lm-ScrollBar {
  display: flex;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


/* <DEPRECATED> */
.p-ScrollBar[data-orientation='horizontal'],
/*  */
.lm-ScrollBar[data-orientation='horizontal'] {
  flex-direction: row;
}


/* <DEPRECATED> */
.p-ScrollBar[data-orientation='vertical'],
/*  */
.lm-ScrollBar[data-orientation='vertical'] {
  flex-direction: column;
}


/* <DEPRECATED> */ .p-ScrollBar-button, /*  */
.lm-ScrollBar-button {
  box-sizing: border-box;
  flex: 0 0 auto;
}


/* <DEPRECATED> */ .p-ScrollBar-track, /*  */
.lm-ScrollBar-track {
  box-sizing: border-box;
  position: relative;
  overflow: hidden;
  flex: 1 1 auto;
}


/* <DEPRECATED> */ .p-ScrollBar-thumb, /*  */
.lm-ScrollBar-thumb {
  box-sizing: border-box;
  position: absolute;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-SplitPanel-child, /*  */
.lm-SplitPanel-child {
  z-index: 0;
}


/* <DEPRECATED> */ .p-SplitPanel-handle, /*  */
.lm-SplitPanel-handle {
  z-index: 1;
}


/* <DEPRECATED> */ .p-SplitPanel-handle.p-mod-hidden, /*  */
.lm-SplitPanel-handle.lm-mod-hidden {
  display: none !important;
}


/* <DEPRECATED> */ .p-SplitPanel-handle:after, /*  */
.lm-SplitPanel-handle:after {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  content: '';
}


/* <DEPRECATED> */
.p-SplitPanel[data-orientation='horizontal'] > .p-SplitPanel-handle,
/*  */
.lm-SplitPanel[data-orientation='horizontal'] > .lm-SplitPanel-handle {
  cursor: ew-resize;
}


/* <DEPRECATED> */
.p-SplitPanel[data-orientation='vertical'] > .p-SplitPanel-handle,
/*  */
.lm-SplitPanel[data-orientation='vertical'] > .lm-SplitPanel-handle {
  cursor: ns-resize;
}


/* <DEPRECATED> */
.p-SplitPanel[data-orientation='horizontal'] > .p-SplitPanel-handle:after,
/*  */
.lm-SplitPanel[data-orientation='horizontal'] > .lm-SplitPanel-handle:after {
  left: 50%;
  min-width: 8px;
  transform: translateX(-50%);
}


/* <DEPRECATED> */
.p-SplitPanel[data-orientation='vertical'] > .p-SplitPanel-handle:after,
/*  */
.lm-SplitPanel[data-orientation='vertical'] > .lm-SplitPanel-handle:after {
  top: 50%;
  min-height: 8px;
  transform: translateY(-50%);
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-TabBar, /*  */
.lm-TabBar {
  display: flex;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


/* <DEPRECATED> */ .p-TabBar[data-orientation='horizontal'], /*  */
.lm-TabBar[data-orientation='horizontal'] {
  flex-direction: row;
  align-items: flex-end;
}


/* <DEPRECATED> */ .p-TabBar[data-orientation='vertical'], /*  */
.lm-TabBar[data-orientation='vertical'] {
  flex-direction: column;
  align-items: flex-end;
}


/* <DEPRECATED> */ .p-TabBar-content, /*  */
.lm-TabBar-content {
  margin: 0;
  padding: 0;
  display: flex;
  flex: 1 1 auto;
  list-style-type: none;
}


/* <DEPRECATED> */
.p-TabBar[data-orientation='horizontal'] > .p-TabBar-content,
/*  */
.lm-TabBar[data-orientation='horizontal'] > .lm-TabBar-content {
  flex-direction: row;
}


/* <DEPRECATED> */
.p-TabBar[data-orientation='vertical'] > .p-TabBar-content,
/*  */
.lm-TabBar[data-orientation='vertical'] > .lm-TabBar-content {
  flex-direction: column;
}


/* <DEPRECATED> */ .p-TabBar-tab, /*  */
.lm-TabBar-tab {
  display: flex;
  flex-direction: row;
  box-sizing: border-box;
  overflow: hidden;
}


/* <DEPRECATED> */
.p-TabBar-tabIcon,
.p-TabBar-tabCloseIcon,
/*  */
.lm-TabBar-tabIcon,
.lm-TabBar-tabCloseIcon {
  flex: 0 0 auto;
}


/* <DEPRECATED> */ .p-TabBar-tabLabel, /*  */
.lm-TabBar-tabLabel {
  flex: 1 1 auto;
  overflow: hidden;
  white-space: nowrap;
}


.lm-TabBar-tabInput {
  user-select: all;
  width: 100%;
  box-sizing : border-box;
}


/* <DEPRECATED> */ .p-TabBar-tab.p-mod-hidden, /*  */
.lm-TabBar-tab.lm-mod-hidden {
  display: none !important;
}


.lm-TabBar-addButton.lm-mod-hidden {
  display: none !important;
}


/* <DEPRECATED> */ .p-TabBar.p-mod-dragging .p-TabBar-tab, /*  */
.lm-TabBar.lm-mod-dragging .lm-TabBar-tab {
  position: relative;
}


/* <DEPRECATED> */
.p-TabBar.p-mod-dragging[data-orientation='horizontal'] .p-TabBar-tab,
/*  */
.lm-TabBar.lm-mod-dragging[data-orientation='horizontal'] .lm-TabBar-tab {
  left: 0;
  transition: left 150ms ease;
}


/* <DEPRECATED> */
.p-TabBar.p-mod-dragging[data-orientation='vertical'] .p-TabBar-tab,
/*  */
.lm-TabBar.lm-mod-dragging[data-orientation='vertical'] .lm-TabBar-tab {
  top: 0;
  transition: top 150ms ease;
}


/* <DEPRECATED> */
.p-TabBar.p-mod-dragging .p-TabBar-tab.p-mod-dragging,
/*  */
.lm-TabBar.lm-mod-dragging .lm-TabBar-tab.lm-mod-dragging {
  transition: none;
}

.lm-TabBar-tabLabel .lm-TabBar-tabInput {
  user-select: all;
  width: 100%;
  box-sizing : border-box;
  background: inherit;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ .p-TabPanel-tabBar, /*  */
.lm-TabPanel-tabBar {
  z-index: 1;
}


/* <DEPRECATED> */ .p-TabPanel-stackedPanel, /*  */
.lm-TabPanel-stackedPanel {
  z-index: 0;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/

@charset "UTF-8";
html{
  -webkit-box-sizing:border-box;
          box-sizing:border-box; }

*,
*::before,
*::after{
  -webkit-box-sizing:inherit;
          box-sizing:inherit; }

body{
  font-size:14px;
  font-weight:400;
  letter-spacing:0;
  line-height:1.28581;
  text-transform:none;
  color:#182026;
  font-family:-apple-system, "BlinkMacSystemFont", "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Open Sans", "Helvetica Neue", "Icons16", sans-serif; }

p{
  margin-bottom:10px;
  margin-top:0; }

small{
  font-size:12px; }

strong{
  font-weight:600; }

::-moz-selection{
  background:rgba(125, 188, 255, 0.6); }

::selection{
  background:rgba(125, 188, 255, 0.6); }
.bp3-heading{
  color:#182026;
  font-weight:600;
  margin:0 0 10px;
  padding:0; }
  .bp3-dark .bp3-heading{
    color:#f5f8fa; }

h1.bp3-heading, .bp3-running-text h1{
  font-size:36px;
  line-height:40px; }

h2.bp3-heading, .bp3-running-text h2{
  font-size:28px;
  line-height:32px; }

h3.bp3-heading, .bp3-running-text h3{
  font-size:22px;
  line-height:25px; }

h4.bp3-heading, .bp3-running-text h4{
  font-size:18px;
  line-height:21px; }

h5.bp3-heading, .bp3-running-text h5{
  font-size:16px;
  line-height:19px; }

h6.bp3-heading, .bp3-running-text h6{
  font-size:14px;
  line-height:16px; }
.bp3-ui-text{
  font-size:14px;
  font-weight:400;
  letter-spacing:0;
  line-height:1.28581;
  text-transform:none; }

.bp3-monospace-text{
  font-family:monospace;
  text-transform:none; }

.bp3-text-muted{
  color:#5c7080; }
  .bp3-dark .bp3-text-muted{
    color:#a7b6c2; }

.bp3-text-disabled{
  color:rgba(92, 112, 128, 0.6); }
  .bp3-dark .bp3-text-disabled{
    color:rgba(167, 182, 194, 0.6); }

.bp3-text-overflow-ellipsis{
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  word-wrap:normal; }
.bp3-running-text{
  font-size:14px;
  line-height:1.5; }
  .bp3-running-text h1{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h1{
      color:#f5f8fa; }
  .bp3-running-text h2{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h2{
      color:#f5f8fa; }
  .bp3-running-text h3{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h3{
      color:#f5f8fa; }
  .bp3-running-text h4{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h4{
      color:#f5f8fa; }
  .bp3-running-text h5{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h5{
      color:#f5f8fa; }
  .bp3-running-text h6{
    color:#182026;
    font-weight:600;
    margin-bottom:20px;
    margin-top:40px; }
    .bp3-dark .bp3-running-text h6{
      color:#f5f8fa; }
  .bp3-running-text hr{
    border:none;
    border-bottom:1px solid rgba(16, 22, 26, 0.15);
    margin:20px 0; }
    .bp3-dark .bp3-running-text hr{
      border-color:rgba(255, 255, 255, 0.15); }
  .bp3-running-text p{
    margin:0 0 10px;
    padding:0; }

.bp3-text-large{
  font-size:16px; }

.bp3-text-small{
  font-size:12px; }
a{
  color:#106ba3;
  text-decoration:none; }
  a:hover{
    color:#106ba3;
    cursor:pointer;
    text-decoration:underline; }
  a .bp3-icon, a .bp3-icon-standard, a .bp3-icon-large{
    color:inherit; }
  a code,
  .bp3-dark a code{
    color:inherit; }
  .bp3-dark a,
  .bp3-dark a:hover{
    color:#48aff0; }
    .bp3-dark a .bp3-icon, .bp3-dark a .bp3-icon-standard, .bp3-dark a .bp3-icon-large,
    .bp3-dark a:hover .bp3-icon,
    .bp3-dark a:hover .bp3-icon-standard,
    .bp3-dark a:hover .bp3-icon-large{
      color:inherit; }
.bp3-running-text code, .bp3-code{
  font-family:monospace;
  text-transform:none;
  background:rgba(255, 255, 255, 0.7);
  border-radius:3px;
  -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2);
          box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2);
  color:#5c7080;
  font-size:smaller;
  padding:2px 5px; }
  .bp3-dark .bp3-running-text code, .bp3-running-text .bp3-dark code, .bp3-dark .bp3-code{
    background:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
    color:#a7b6c2; }
  .bp3-running-text a > code, a > .bp3-code{
    color:#137cbd; }
    .bp3-dark .bp3-running-text a > code, .bp3-running-text .bp3-dark a > code, .bp3-dark a > .bp3-code{
      color:inherit; }

.bp3-running-text pre, .bp3-code-block{
  font-family:monospace;
  text-transform:none;
  background:rgba(255, 255, 255, 0.7);
  border-radius:3px;
  -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15);
          box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15);
  color:#182026;
  display:block;
  font-size:13px;
  line-height:1.4;
  margin:10px 0;
  padding:13px 15px 12px;
  word-break:break-all;
  word-wrap:break-word; }
  .bp3-dark .bp3-running-text pre, .bp3-running-text .bp3-dark pre, .bp3-dark .bp3-code-block{
    background:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }
  .bp3-running-text pre > code, .bp3-code-block > code{
    background:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    color:inherit;
    font-size:inherit;
    padding:0; }

.bp3-running-text kbd, .bp3-key{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  background:#ffffff;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
  color:#5c7080;
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex;
  font-family:inherit;
  font-size:12px;
  height:24px;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  line-height:24px;
  min-width:24px;
  padding:3px 6px;
  vertical-align:middle; }
  .bp3-running-text kbd .bp3-icon, .bp3-key .bp3-icon, .bp3-running-text kbd .bp3-icon-standard, .bp3-key .bp3-icon-standard, .bp3-running-text kbd .bp3-icon-large, .bp3-key .bp3-icon-large{
    margin-right:5px; }
  .bp3-dark .bp3-running-text kbd, .bp3-running-text .bp3-dark kbd, .bp3-dark .bp3-key{
    background:#394b59;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
    color:#a7b6c2; }
.bp3-running-text blockquote, .bp3-blockquote{
  border-left:solid 4px rgba(167, 182, 194, 0.5);
  margin:0 0 10px;
  padding:0 20px; }
  .bp3-dark .bp3-running-text blockquote, .bp3-running-text .bp3-dark blockquote, .bp3-dark .bp3-blockquote{
    border-color:rgba(115, 134, 148, 0.5); }
.bp3-running-text ul,
.bp3-running-text ol, .bp3-list{
  margin:10px 0;
  padding-left:30px; }
  .bp3-running-text ul li:not(:last-child), .bp3-running-text ol li:not(:last-child), .bp3-list li:not(:last-child){
    margin-bottom:5px; }
  .bp3-running-text ul ol, .bp3-running-text ol ol, .bp3-list ol,
  .bp3-running-text ul ul,
  .bp3-running-text ol ul,
  .bp3-list ul{
    margin-top:5px; }

.bp3-list-unstyled{
  list-style:none;
  margin:0;
  padding:0; }
  .bp3-list-unstyled li{
    padding:0; }
.bp3-rtl{
  text-align:right; }

.bp3-dark{
  color:#f5f8fa; }

:focus{
  outline:rgba(19, 124, 189, 0.6) auto 2px;
  outline-offset:2px;
  -moz-outline-radius:6px; }

.bp3-focus-disabled :focus{
  outline:none !important; }
  .bp3-focus-disabled :focus ~ .bp3-control-indicator{
    outline:none !important; }

.bp3-alert{
  max-width:400px;
  padding:20px; }

.bp3-alert-body{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex; }
  .bp3-alert-body .bp3-icon{
    font-size:40px;
    margin-right:20px;
    margin-top:0; }

.bp3-alert-contents{
  word-break:break-word; }

.bp3-alert-footer{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:reverse;
      -ms-flex-direction:row-reverse;
          flex-direction:row-reverse;
  margin-top:10px; }
  .bp3-alert-footer .bp3-button{
    margin-left:10px; }
.bp3-breadcrumbs{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  cursor:default;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -ms-flex-wrap:wrap;
      flex-wrap:wrap;
  height:30px;
  list-style:none;
  margin:0;
  padding:0; }
  .bp3-breadcrumbs > li{
    -webkit-box-align:center;
        -ms-flex-align:center;
            align-items:center;
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex; }
    .bp3-breadcrumbs > li::after{
      background:url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3e%3cpath fill-rule='evenodd' clip-rule='evenodd' d='M10.71 7.29l-4-4a1.003 1.003 0 00-1.42 1.42L8.59 8 5.3 11.29c-.19.18-.3.43-.3.71a1.003 1.003 0 001.71.71l4-4c.18-.18.29-.43.29-.71 0-.28-.11-.53-.29-.71z' fill='%235C7080'/%3e%3c/svg%3e");
      content:"";
      display:block;
      height:16px;
      margin:0 5px;
      width:16px; }
    .bp3-breadcrumbs > li:last-of-type::after{
      display:none; }

.bp3-breadcrumb,
.bp3-breadcrumb-current,
.bp3-breadcrumbs-collapsed{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex;
  font-size:16px; }

.bp3-breadcrumb,
.bp3-breadcrumbs-collapsed{
  color:#5c7080; }

.bp3-breadcrumb:hover{
  text-decoration:none; }

.bp3-breadcrumb.bp3-disabled{
  color:rgba(92, 112, 128, 0.6);
  cursor:not-allowed; }

.bp3-breadcrumb .bp3-icon{
  margin-right:5px; }

.bp3-breadcrumb-current{
  color:inherit;
  font-weight:600; }
  .bp3-breadcrumb-current .bp3-input{
    font-size:inherit;
    font-weight:inherit;
    vertical-align:baseline; }

.bp3-breadcrumbs-collapsed{
  background:#ced9e0;
  border:none;
  border-radius:3px;
  cursor:pointer;
  margin-right:2px;
  padding:1px 5px;
  vertical-align:text-bottom; }
  .bp3-breadcrumbs-collapsed::before{
    background:url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3e%3cg fill='%235C7080'%3e%3ccircle cx='2' cy='8.03' r='2'/%3e%3ccircle cx='14' cy='8.03' r='2'/%3e%3ccircle cx='8' cy='8.03' r='2'/%3e%3c/g%3e%3c/svg%3e") center no-repeat;
    content:"";
    display:block;
    height:16px;
    width:16px; }
  .bp3-breadcrumbs-collapsed:hover{
    background:#bfccd6;
    color:#182026;
    text-decoration:none; }

.bp3-dark .bp3-breadcrumb,
.bp3-dark .bp3-breadcrumbs-collapsed{
  color:#a7b6c2; }

.bp3-dark .bp3-breadcrumbs > li::after{
  color:#a7b6c2; }

.bp3-dark .bp3-breadcrumb.bp3-disabled{
  color:rgba(167, 182, 194, 0.6); }

.bp3-dark .bp3-breadcrumb-current{
  color:#f5f8fa; }

.bp3-dark .bp3-breadcrumbs-collapsed{
  background:rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-breadcrumbs-collapsed:hover{
    background:rgba(16, 22, 26, 0.6);
    color:#f5f8fa; }
.bp3-button{
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  border:none;
  border-radius:3px;
  cursor:pointer;
  font-size:14px;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  padding:5px 10px;
  text-align:left;
  vertical-align:middle;
  min-height:30px;
  min-width:30px; }
  .bp3-button > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-button > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-button::before,
  .bp3-button > *{
    margin-right:7px; }
  .bp3-button:empty::before,
  .bp3-button > :last-child{
    margin-right:0; }
  .bp3-button:empty{
    padding:0 !important; }
  .bp3-button:disabled, .bp3-button.bp3-disabled{
    cursor:not-allowed; }
  .bp3-button.bp3-fill{
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    width:100%; }
  .bp3-button.bp3-align-right,
  .bp3-align-right .bp3-button{
    text-align:right; }
  .bp3-button.bp3-align-left,
  .bp3-align-left .bp3-button{
    text-align:left; }
  .bp3-button:not([class*="bp3-intent-"]){
    background-color:#f5f8fa;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.8)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
    color:#182026; }
    .bp3-button:not([class*="bp3-intent-"]):hover{
      background-clip:padding-box;
      background-color:#ebf1f5;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
    .bp3-button:not([class*="bp3-intent-"]):active, .bp3-button:not([class*="bp3-intent-"]).bp3-active{
      background-color:#d8e1e8;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-button:not([class*="bp3-intent-"]):disabled, .bp3-button:not([class*="bp3-intent-"]).bp3-disabled{
      background-color:rgba(206, 217, 224, 0.5);
      background-image:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed;
      outline:none; }
      .bp3-button:not([class*="bp3-intent-"]):disabled.bp3-active, .bp3-button:not([class*="bp3-intent-"]):disabled.bp3-active:hover, .bp3-button:not([class*="bp3-intent-"]).bp3-disabled.bp3-active, .bp3-button:not([class*="bp3-intent-"]).bp3-disabled.bp3-active:hover{
        background:rgba(206, 217, 224, 0.7); }
  .bp3-button.bp3-intent-primary{
    background-color:#137cbd;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
    .bp3-button.bp3-intent-primary:hover, .bp3-button.bp3-intent-primary:active, .bp3-button.bp3-intent-primary.bp3-active{
      color:#ffffff; }
    .bp3-button.bp3-intent-primary:hover{
      background-color:#106ba3;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-primary:active, .bp3-button.bp3-intent-primary.bp3-active{
      background-color:#0e5a8a;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-primary:disabled, .bp3-button.bp3-intent-primary.bp3-disabled{
      background-color:rgba(19, 124, 189, 0.5);
      background-image:none;
      border-color:transparent;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(255, 255, 255, 0.6); }
  .bp3-button.bp3-intent-success{
    background-color:#0f9960;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
    .bp3-button.bp3-intent-success:hover, .bp3-button.bp3-intent-success:active, .bp3-button.bp3-intent-success.bp3-active{
      color:#ffffff; }
    .bp3-button.bp3-intent-success:hover{
      background-color:#0d8050;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-success:active, .bp3-button.bp3-intent-success.bp3-active{
      background-color:#0a6640;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-success:disabled, .bp3-button.bp3-intent-success.bp3-disabled{
      background-color:rgba(15, 153, 96, 0.5);
      background-image:none;
      border-color:transparent;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(255, 255, 255, 0.6); }
  .bp3-button.bp3-intent-warning{
    background-color:#d9822b;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
    .bp3-button.bp3-intent-warning:hover, .bp3-button.bp3-intent-warning:active, .bp3-button.bp3-intent-warning.bp3-active{
      color:#ffffff; }
    .bp3-button.bp3-intent-warning:hover{
      background-color:#bf7326;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-warning:active, .bp3-button.bp3-intent-warning.bp3-active{
      background-color:#a66321;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-warning:disabled, .bp3-button.bp3-intent-warning.bp3-disabled{
      background-color:rgba(217, 130, 43, 0.5);
      background-image:none;
      border-color:transparent;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(255, 255, 255, 0.6); }
  .bp3-button.bp3-intent-danger{
    background-color:#db3737;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
    .bp3-button.bp3-intent-danger:hover, .bp3-button.bp3-intent-danger:active, .bp3-button.bp3-intent-danger.bp3-active{
      color:#ffffff; }
    .bp3-button.bp3-intent-danger:hover{
      background-color:#c23030;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-danger:active, .bp3-button.bp3-intent-danger.bp3-active{
      background-color:#a82a2a;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-button.bp3-intent-danger:disabled, .bp3-button.bp3-intent-danger.bp3-disabled{
      background-color:rgba(219, 55, 55, 0.5);
      background-image:none;
      border-color:transparent;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(255, 255, 255, 0.6); }
  .bp3-button[class*="bp3-intent-"] .bp3-button-spinner .bp3-spinner-head{
    stroke:#ffffff; }
  .bp3-button.bp3-large,
  .bp3-large .bp3-button{
    min-height:40px;
    min-width:40px;
    font-size:16px;
    padding:5px 15px; }
    .bp3-button.bp3-large::before,
    .bp3-button.bp3-large > *,
    .bp3-large .bp3-button::before,
    .bp3-large .bp3-button > *{
      margin-right:10px; }
    .bp3-button.bp3-large:empty::before,
    .bp3-button.bp3-large > :last-child,
    .bp3-large .bp3-button:empty::before,
    .bp3-large .bp3-button > :last-child{
      margin-right:0; }
  .bp3-button.bp3-small,
  .bp3-small .bp3-button{
    min-height:24px;
    min-width:24px;
    padding:0 7px; }
  .bp3-button.bp3-loading{
    position:relative; }
    .bp3-button.bp3-loading[class*="bp3-icon-"]::before{
      visibility:hidden; }
    .bp3-button.bp3-loading .bp3-button-spinner{
      margin:0;
      position:absolute; }
    .bp3-button.bp3-loading > :not(.bp3-button-spinner){
      visibility:hidden; }
  .bp3-button[class*="bp3-icon-"]::before{
    font-family:"Icons16", sans-serif;
    font-size:16px;
    font-style:normal;
    font-weight:400;
    line-height:1;
    -moz-osx-font-smoothing:grayscale;
    -webkit-font-smoothing:antialiased;
    color:#5c7080; }
  .bp3-button .bp3-icon, .bp3-button .bp3-icon-standard, .bp3-button .bp3-icon-large{
    color:#5c7080; }
    .bp3-button .bp3-icon.bp3-align-right, .bp3-button .bp3-icon-standard.bp3-align-right, .bp3-button .bp3-icon-large.bp3-align-right{
      margin-left:7px; }
  .bp3-button .bp3-icon:first-child:last-child,
  .bp3-button .bp3-spinner + .bp3-icon:last-child{
    margin:0 -7px; }
  .bp3-dark .bp3-button:not([class*="bp3-intent-"]){
    background-color:#394b59;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.05)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]):hover, .bp3-dark .bp3-button:not([class*="bp3-intent-"]):active, .bp3-dark .bp3-button:not([class*="bp3-intent-"]).bp3-active{
      color:#f5f8fa; }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]):hover{
      background-color:#30404d;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]):active, .bp3-dark .bp3-button:not([class*="bp3-intent-"]).bp3-active{
      background-color:#202b33;
      background-image:none;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]):disabled, .bp3-dark .bp3-button:not([class*="bp3-intent-"]).bp3-disabled{
      background-color:rgba(57, 75, 89, 0.5);
      background-image:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(167, 182, 194, 0.6); }
      .bp3-dark .bp3-button:not([class*="bp3-intent-"]):disabled.bp3-active, .bp3-dark .bp3-button:not([class*="bp3-intent-"]).bp3-disabled.bp3-active{
        background:rgba(57, 75, 89, 0.7); }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]) .bp3-button-spinner .bp3-spinner-head{
      background:rgba(16, 22, 26, 0.5);
      stroke:#8a9ba8; }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"])[class*="bp3-icon-"]::before{
      color:#a7b6c2; }
    .bp3-dark .bp3-button:not([class*="bp3-intent-"]) .bp3-icon, .bp3-dark .bp3-button:not([class*="bp3-intent-"]) .bp3-icon-standard, .bp3-dark .bp3-button:not([class*="bp3-intent-"]) .bp3-icon-large{
      color:#a7b6c2; }
  .bp3-dark .bp3-button[class*="bp3-intent-"]{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-button[class*="bp3-intent-"]:hover{
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-button[class*="bp3-intent-"]:active, .bp3-dark .bp3-button[class*="bp3-intent-"].bp3-active{
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-dark .bp3-button[class*="bp3-intent-"]:disabled, .bp3-dark .bp3-button[class*="bp3-intent-"].bp3-disabled{
      background-image:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(255, 255, 255, 0.3); }
    .bp3-dark .bp3-button[class*="bp3-intent-"] .bp3-button-spinner .bp3-spinner-head{
      stroke:#8a9ba8; }
  .bp3-button:disabled::before,
  .bp3-button:disabled .bp3-icon, .bp3-button:disabled .bp3-icon-standard, .bp3-button:disabled .bp3-icon-large, .bp3-button.bp3-disabled::before,
  .bp3-button.bp3-disabled .bp3-icon, .bp3-button.bp3-disabled .bp3-icon-standard, .bp3-button.bp3-disabled .bp3-icon-large, .bp3-button[class*="bp3-intent-"]::before,
  .bp3-button[class*="bp3-intent-"] .bp3-icon, .bp3-button[class*="bp3-intent-"] .bp3-icon-standard, .bp3-button[class*="bp3-intent-"] .bp3-icon-large{
    color:inherit !important; }
  .bp3-button.bp3-minimal{
    background:none;
    -webkit-box-shadow:none;
            box-shadow:none; }
    .bp3-button.bp3-minimal:hover{
      background:rgba(167, 182, 194, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026;
      text-decoration:none; }
    .bp3-button.bp3-minimal:active, .bp3-button.bp3-minimal.bp3-active{
      background:rgba(115, 134, 148, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026; }
    .bp3-button.bp3-minimal:disabled, .bp3-button.bp3-minimal:disabled:hover, .bp3-button.bp3-minimal.bp3-disabled, .bp3-button.bp3-minimal.bp3-disabled:hover{
      background:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed; }
      .bp3-button.bp3-minimal:disabled.bp3-active, .bp3-button.bp3-minimal:disabled:hover.bp3-active, .bp3-button.bp3-minimal.bp3-disabled.bp3-active, .bp3-button.bp3-minimal.bp3-disabled:hover.bp3-active{
        background:rgba(115, 134, 148, 0.3); }
    .bp3-dark .bp3-button.bp3-minimal{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:inherit; }
      .bp3-dark .bp3-button.bp3-minimal:hover, .bp3-dark .bp3-button.bp3-minimal:active, .bp3-dark .bp3-button.bp3-minimal.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none; }
      .bp3-dark .bp3-button.bp3-minimal:hover{
        background:rgba(138, 155, 168, 0.15); }
      .bp3-dark .bp3-button.bp3-minimal:active, .bp3-dark .bp3-button.bp3-minimal.bp3-active{
        background:rgba(138, 155, 168, 0.3);
        color:#f5f8fa; }
      .bp3-dark .bp3-button.bp3-minimal:disabled, .bp3-dark .bp3-button.bp3-minimal:disabled:hover, .bp3-dark .bp3-button.bp3-minimal.bp3-disabled, .bp3-dark .bp3-button.bp3-minimal.bp3-disabled:hover{
        background:none;
        color:rgba(167, 182, 194, 0.6);
        cursor:not-allowed; }
        .bp3-dark .bp3-button.bp3-minimal:disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal:disabled:hover.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-disabled:hover.bp3-active{
          background:rgba(138, 155, 168, 0.3); }
    .bp3-button.bp3-minimal.bp3-intent-primary{
      color:#106ba3; }
      .bp3-button.bp3-minimal.bp3-intent-primary:hover, .bp3-button.bp3-minimal.bp3-intent-primary:active, .bp3-button.bp3-minimal.bp3-intent-primary.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#106ba3; }
      .bp3-button.bp3-minimal.bp3-intent-primary:hover{
        background:rgba(19, 124, 189, 0.15);
        color:#106ba3; }
      .bp3-button.bp3-minimal.bp3-intent-primary:active, .bp3-button.bp3-minimal.bp3-intent-primary.bp3-active{
        background:rgba(19, 124, 189, 0.3);
        color:#106ba3; }
      .bp3-button.bp3-minimal.bp3-intent-primary:disabled, .bp3-button.bp3-minimal.bp3-intent-primary.bp3-disabled{
        background:none;
        color:rgba(16, 107, 163, 0.5); }
        .bp3-button.bp3-minimal.bp3-intent-primary:disabled.bp3-active, .bp3-button.bp3-minimal.bp3-intent-primary.bp3-disabled.bp3-active{
          background:rgba(19, 124, 189, 0.3); }
      .bp3-button.bp3-minimal.bp3-intent-primary .bp3-button-spinner .bp3-spinner-head{
        stroke:#106ba3; }
      .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary{
        color:#48aff0; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary:hover{
          background:rgba(19, 124, 189, 0.2);
          color:#48aff0; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary:active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary.bp3-active{
          background:rgba(19, 124, 189, 0.3);
          color:#48aff0; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary:disabled, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary.bp3-disabled{
          background:none;
          color:rgba(72, 175, 240, 0.5); }
          .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary:disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-primary.bp3-disabled.bp3-active{
            background:rgba(19, 124, 189, 0.3); }
    .bp3-button.bp3-minimal.bp3-intent-success{
      color:#0d8050; }
      .bp3-button.bp3-minimal.bp3-intent-success:hover, .bp3-button.bp3-minimal.bp3-intent-success:active, .bp3-button.bp3-minimal.bp3-intent-success.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#0d8050; }
      .bp3-button.bp3-minimal.bp3-intent-success:hover{
        background:rgba(15, 153, 96, 0.15);
        color:#0d8050; }
      .bp3-button.bp3-minimal.bp3-intent-success:active, .bp3-button.bp3-minimal.bp3-intent-success.bp3-active{
        background:rgba(15, 153, 96, 0.3);
        color:#0d8050; }
      .bp3-button.bp3-minimal.bp3-intent-success:disabled, .bp3-button.bp3-minimal.bp3-intent-success.bp3-disabled{
        background:none;
        color:rgba(13, 128, 80, 0.5); }
        .bp3-button.bp3-minimal.bp3-intent-success:disabled.bp3-active, .bp3-button.bp3-minimal.bp3-intent-success.bp3-disabled.bp3-active{
          background:rgba(15, 153, 96, 0.3); }
      .bp3-button.bp3-minimal.bp3-intent-success .bp3-button-spinner .bp3-spinner-head{
        stroke:#0d8050; }
      .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success{
        color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success:hover{
          background:rgba(15, 153, 96, 0.2);
          color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success:active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success.bp3-active{
          background:rgba(15, 153, 96, 0.3);
          color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success:disabled, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success.bp3-disabled{
          background:none;
          color:rgba(61, 204, 145, 0.5); }
          .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success:disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-success.bp3-disabled.bp3-active{
            background:rgba(15, 153, 96, 0.3); }
    .bp3-button.bp3-minimal.bp3-intent-warning{
      color:#bf7326; }
      .bp3-button.bp3-minimal.bp3-intent-warning:hover, .bp3-button.bp3-minimal.bp3-intent-warning:active, .bp3-button.bp3-minimal.bp3-intent-warning.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#bf7326; }
      .bp3-button.bp3-minimal.bp3-intent-warning:hover{
        background:rgba(217, 130, 43, 0.15);
        color:#bf7326; }
      .bp3-button.bp3-minimal.bp3-intent-warning:active, .bp3-button.bp3-minimal.bp3-intent-warning.bp3-active{
        background:rgba(217, 130, 43, 0.3);
        color:#bf7326; }
      .bp3-button.bp3-minimal.bp3-intent-warning:disabled, .bp3-button.bp3-minimal.bp3-intent-warning.bp3-disabled{
        background:none;
        color:rgba(191, 115, 38, 0.5); }
        .bp3-button.bp3-minimal.bp3-intent-warning:disabled.bp3-active, .bp3-button.bp3-minimal.bp3-intent-warning.bp3-disabled.bp3-active{
          background:rgba(217, 130, 43, 0.3); }
      .bp3-button.bp3-minimal.bp3-intent-warning .bp3-button-spinner .bp3-spinner-head{
        stroke:#bf7326; }
      .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning{
        color:#ffb366; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning:hover{
          background:rgba(217, 130, 43, 0.2);
          color:#ffb366; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning:active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning.bp3-active{
          background:rgba(217, 130, 43, 0.3);
          color:#ffb366; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning:disabled, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning.bp3-disabled{
          background:none;
          color:rgba(255, 179, 102, 0.5); }
          .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning:disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-warning.bp3-disabled.bp3-active{
            background:rgba(217, 130, 43, 0.3); }
    .bp3-button.bp3-minimal.bp3-intent-danger{
      color:#c23030; }
      .bp3-button.bp3-minimal.bp3-intent-danger:hover, .bp3-button.bp3-minimal.bp3-intent-danger:active, .bp3-button.bp3-minimal.bp3-intent-danger.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#c23030; }
      .bp3-button.bp3-minimal.bp3-intent-danger:hover{
        background:rgba(219, 55, 55, 0.15);
        color:#c23030; }
      .bp3-button.bp3-minimal.bp3-intent-danger:active, .bp3-button.bp3-minimal.bp3-intent-danger.bp3-active{
        background:rgba(219, 55, 55, 0.3);
        color:#c23030; }
      .bp3-button.bp3-minimal.bp3-intent-danger:disabled, .bp3-button.bp3-minimal.bp3-intent-danger.bp3-disabled{
        background:none;
        color:rgba(194, 48, 48, 0.5); }
        .bp3-button.bp3-minimal.bp3-intent-danger:disabled.bp3-active, .bp3-button.bp3-minimal.bp3-intent-danger.bp3-disabled.bp3-active{
          background:rgba(219, 55, 55, 0.3); }
      .bp3-button.bp3-minimal.bp3-intent-danger .bp3-button-spinner .bp3-spinner-head{
        stroke:#c23030; }
      .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger{
        color:#ff7373; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger:hover{
          background:rgba(219, 55, 55, 0.2);
          color:#ff7373; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger:active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger.bp3-active{
          background:rgba(219, 55, 55, 0.3);
          color:#ff7373; }
        .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger:disabled, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger.bp3-disabled{
          background:none;
          color:rgba(255, 115, 115, 0.5); }
          .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger:disabled.bp3-active, .bp3-dark .bp3-button.bp3-minimal.bp3-intent-danger.bp3-disabled.bp3-active{
            background:rgba(219, 55, 55, 0.3); }
  .bp3-button.bp3-outlined{
    background:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    border:1px solid rgba(24, 32, 38, 0.2);
    -webkit-box-sizing:border-box;
            box-sizing:border-box; }
    .bp3-button.bp3-outlined:hover{
      background:rgba(167, 182, 194, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026;
      text-decoration:none; }
    .bp3-button.bp3-outlined:active, .bp3-button.bp3-outlined.bp3-active{
      background:rgba(115, 134, 148, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026; }
    .bp3-button.bp3-outlined:disabled, .bp3-button.bp3-outlined:disabled:hover, .bp3-button.bp3-outlined.bp3-disabled, .bp3-button.bp3-outlined.bp3-disabled:hover{
      background:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed; }
      .bp3-button.bp3-outlined:disabled.bp3-active, .bp3-button.bp3-outlined:disabled:hover.bp3-active, .bp3-button.bp3-outlined.bp3-disabled.bp3-active, .bp3-button.bp3-outlined.bp3-disabled:hover.bp3-active{
        background:rgba(115, 134, 148, 0.3); }
    .bp3-dark .bp3-button.bp3-outlined{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:inherit; }
      .bp3-dark .bp3-button.bp3-outlined:hover, .bp3-dark .bp3-button.bp3-outlined:active, .bp3-dark .bp3-button.bp3-outlined.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none; }
      .bp3-dark .bp3-button.bp3-outlined:hover{
        background:rgba(138, 155, 168, 0.15); }
      .bp3-dark .bp3-button.bp3-outlined:active, .bp3-dark .bp3-button.bp3-outlined.bp3-active{
        background:rgba(138, 155, 168, 0.3);
        color:#f5f8fa; }
      .bp3-dark .bp3-button.bp3-outlined:disabled, .bp3-dark .bp3-button.bp3-outlined:disabled:hover, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled:hover{
        background:none;
        color:rgba(167, 182, 194, 0.6);
        cursor:not-allowed; }
        .bp3-dark .bp3-button.bp3-outlined:disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined:disabled:hover.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled:hover.bp3-active{
          background:rgba(138, 155, 168, 0.3); }
    .bp3-button.bp3-outlined.bp3-intent-primary{
      color:#106ba3; }
      .bp3-button.bp3-outlined.bp3-intent-primary:hover, .bp3-button.bp3-outlined.bp3-intent-primary:active, .bp3-button.bp3-outlined.bp3-intent-primary.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#106ba3; }
      .bp3-button.bp3-outlined.bp3-intent-primary:hover{
        background:rgba(19, 124, 189, 0.15);
        color:#106ba3; }
      .bp3-button.bp3-outlined.bp3-intent-primary:active, .bp3-button.bp3-outlined.bp3-intent-primary.bp3-active{
        background:rgba(19, 124, 189, 0.3);
        color:#106ba3; }
      .bp3-button.bp3-outlined.bp3-intent-primary:disabled, .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled{
        background:none;
        color:rgba(16, 107, 163, 0.5); }
        .bp3-button.bp3-outlined.bp3-intent-primary:disabled.bp3-active, .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled.bp3-active{
          background:rgba(19, 124, 189, 0.3); }
      .bp3-button.bp3-outlined.bp3-intent-primary .bp3-button-spinner .bp3-spinner-head{
        stroke:#106ba3; }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary{
        color:#48aff0; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary:hover{
          background:rgba(19, 124, 189, 0.2);
          color:#48aff0; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary:active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary.bp3-active{
          background:rgba(19, 124, 189, 0.3);
          color:#48aff0; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled{
          background:none;
          color:rgba(72, 175, 240, 0.5); }
          .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary:disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled.bp3-active{
            background:rgba(19, 124, 189, 0.3); }
    .bp3-button.bp3-outlined.bp3-intent-success{
      color:#0d8050; }
      .bp3-button.bp3-outlined.bp3-intent-success:hover, .bp3-button.bp3-outlined.bp3-intent-success:active, .bp3-button.bp3-outlined.bp3-intent-success.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#0d8050; }
      .bp3-button.bp3-outlined.bp3-intent-success:hover{
        background:rgba(15, 153, 96, 0.15);
        color:#0d8050; }
      .bp3-button.bp3-outlined.bp3-intent-success:active, .bp3-button.bp3-outlined.bp3-intent-success.bp3-active{
        background:rgba(15, 153, 96, 0.3);
        color:#0d8050; }
      .bp3-button.bp3-outlined.bp3-intent-success:disabled, .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled{
        background:none;
        color:rgba(13, 128, 80, 0.5); }
        .bp3-button.bp3-outlined.bp3-intent-success:disabled.bp3-active, .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled.bp3-active{
          background:rgba(15, 153, 96, 0.3); }
      .bp3-button.bp3-outlined.bp3-intent-success .bp3-button-spinner .bp3-spinner-head{
        stroke:#0d8050; }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success{
        color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success:hover{
          background:rgba(15, 153, 96, 0.2);
          color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success:active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success.bp3-active{
          background:rgba(15, 153, 96, 0.3);
          color:#3dcc91; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled{
          background:none;
          color:rgba(61, 204, 145, 0.5); }
          .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success:disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled.bp3-active{
            background:rgba(15, 153, 96, 0.3); }
    .bp3-button.bp3-outlined.bp3-intent-warning{
      color:#bf7326; }
      .bp3-button.bp3-outlined.bp3-intent-warning:hover, .bp3-button.bp3-outlined.bp3-intent-warning:active, .bp3-button.bp3-outlined.bp3-intent-warning.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#bf7326; }
      .bp3-button.bp3-outlined.bp3-intent-warning:hover{
        background:rgba(217, 130, 43, 0.15);
        color:#bf7326; }
      .bp3-button.bp3-outlined.bp3-intent-warning:active, .bp3-button.bp3-outlined.bp3-intent-warning.bp3-active{
        background:rgba(217, 130, 43, 0.3);
        color:#bf7326; }
      .bp3-button.bp3-outlined.bp3-intent-warning:disabled, .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled{
        background:none;
        color:rgba(191, 115, 38, 0.5); }
        .bp3-button.bp3-outlined.bp3-intent-warning:disabled.bp3-active, .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled.bp3-active{
          background:rgba(217, 130, 43, 0.3); }
      .bp3-button.bp3-outlined.bp3-intent-warning .bp3-button-spinner .bp3-spinner-head{
        stroke:#bf7326; }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning{
        color:#ffb366; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning:hover{
          background:rgba(217, 130, 43, 0.2);
          color:#ffb366; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning:active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning.bp3-active{
          background:rgba(217, 130, 43, 0.3);
          color:#ffb366; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled{
          background:none;
          color:rgba(255, 179, 102, 0.5); }
          .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning:disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled.bp3-active{
            background:rgba(217, 130, 43, 0.3); }
    .bp3-button.bp3-outlined.bp3-intent-danger{
      color:#c23030; }
      .bp3-button.bp3-outlined.bp3-intent-danger:hover, .bp3-button.bp3-outlined.bp3-intent-danger:active, .bp3-button.bp3-outlined.bp3-intent-danger.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#c23030; }
      .bp3-button.bp3-outlined.bp3-intent-danger:hover{
        background:rgba(219, 55, 55, 0.15);
        color:#c23030; }
      .bp3-button.bp3-outlined.bp3-intent-danger:active, .bp3-button.bp3-outlined.bp3-intent-danger.bp3-active{
        background:rgba(219, 55, 55, 0.3);
        color:#c23030; }
      .bp3-button.bp3-outlined.bp3-intent-danger:disabled, .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled{
        background:none;
        color:rgba(194, 48, 48, 0.5); }
        .bp3-button.bp3-outlined.bp3-intent-danger:disabled.bp3-active, .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled.bp3-active{
          background:rgba(219, 55, 55, 0.3); }
      .bp3-button.bp3-outlined.bp3-intent-danger .bp3-button-spinner .bp3-spinner-head{
        stroke:#c23030; }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger{
        color:#ff7373; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger:hover{
          background:rgba(219, 55, 55, 0.2);
          color:#ff7373; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger:active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger.bp3-active{
          background:rgba(219, 55, 55, 0.3);
          color:#ff7373; }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled{
          background:none;
          color:rgba(255, 115, 115, 0.5); }
          .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger:disabled.bp3-active, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled.bp3-active{
            background:rgba(219, 55, 55, 0.3); }
    .bp3-button.bp3-outlined:disabled, .bp3-button.bp3-outlined.bp3-disabled, .bp3-button.bp3-outlined:disabled:hover, .bp3-button.bp3-outlined.bp3-disabled:hover{
      border-color:rgba(92, 112, 128, 0.1); }
    .bp3-dark .bp3-button.bp3-outlined{
      border-color:rgba(255, 255, 255, 0.4); }
      .bp3-dark .bp3-button.bp3-outlined:disabled, .bp3-dark .bp3-button.bp3-outlined:disabled:hover, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-disabled:hover{
        border-color:rgba(255, 255, 255, 0.2); }
    .bp3-button.bp3-outlined.bp3-intent-primary{
      border-color:rgba(16, 107, 163, 0.6); }
      .bp3-button.bp3-outlined.bp3-intent-primary:disabled, .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled{
        border-color:rgba(16, 107, 163, 0.2); }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary{
        border-color:rgba(72, 175, 240, 0.6); }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-primary.bp3-disabled{
          border-color:rgba(72, 175, 240, 0.2); }
    .bp3-button.bp3-outlined.bp3-intent-success{
      border-color:rgba(13, 128, 80, 0.6); }
      .bp3-button.bp3-outlined.bp3-intent-success:disabled, .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled{
        border-color:rgba(13, 128, 80, 0.2); }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success{
        border-color:rgba(61, 204, 145, 0.6); }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-success.bp3-disabled{
          border-color:rgba(61, 204, 145, 0.2); }
    .bp3-button.bp3-outlined.bp3-intent-warning{
      border-color:rgba(191, 115, 38, 0.6); }
      .bp3-button.bp3-outlined.bp3-intent-warning:disabled, .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled{
        border-color:rgba(191, 115, 38, 0.2); }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning{
        border-color:rgba(255, 179, 102, 0.6); }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-warning.bp3-disabled{
          border-color:rgba(255, 179, 102, 0.2); }
    .bp3-button.bp3-outlined.bp3-intent-danger{
      border-color:rgba(194, 48, 48, 0.6); }
      .bp3-button.bp3-outlined.bp3-intent-danger:disabled, .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled{
        border-color:rgba(194, 48, 48, 0.2); }
      .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger{
        border-color:rgba(255, 115, 115, 0.6); }
        .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger:disabled, .bp3-dark .bp3-button.bp3-outlined.bp3-intent-danger.bp3-disabled{
          border-color:rgba(255, 115, 115, 0.2); }

a.bp3-button{
  text-align:center;
  text-decoration:none;
  -webkit-transition:none;
  transition:none; }
  a.bp3-button, a.bp3-button:hover, a.bp3-button:active{
    color:#182026; }
  a.bp3-button.bp3-disabled{
    color:rgba(92, 112, 128, 0.6); }

.bp3-button-text{
  -webkit-box-flex:0;
      -ms-flex:0 1 auto;
          flex:0 1 auto; }

.bp3-button.bp3-align-left .bp3-button-text, .bp3-button.bp3-align-right .bp3-button-text,
.bp3-button-group.bp3-align-left .bp3-button-text,
.bp3-button-group.bp3-align-right .bp3-button-text{
  -webkit-box-flex:1;
      -ms-flex:1 1 auto;
          flex:1 1 auto; }
.bp3-button-group{
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex; }
  .bp3-button-group .bp3-button{
    -webkit-box-flex:0;
        -ms-flex:0 0 auto;
            flex:0 0 auto;
    position:relative;
    z-index:4; }
    .bp3-button-group .bp3-button:focus{
      z-index:5; }
    .bp3-button-group .bp3-button:hover{
      z-index:6; }
    .bp3-button-group .bp3-button:active, .bp3-button-group .bp3-button.bp3-active{
      z-index:7; }
    .bp3-button-group .bp3-button:disabled, .bp3-button-group .bp3-button.bp3-disabled{
      z-index:3; }
    .bp3-button-group .bp3-button[class*="bp3-intent-"]{
      z-index:9; }
      .bp3-button-group .bp3-button[class*="bp3-intent-"]:focus{
        z-index:10; }
      .bp3-button-group .bp3-button[class*="bp3-intent-"]:hover{
        z-index:11; }
      .bp3-button-group .bp3-button[class*="bp3-intent-"]:active, .bp3-button-group .bp3-button[class*="bp3-intent-"].bp3-active{
        z-index:12; }
      .bp3-button-group .bp3-button[class*="bp3-intent-"]:disabled, .bp3-button-group .bp3-button[class*="bp3-intent-"].bp3-disabled{
        z-index:8; }
  .bp3-button-group:not(.bp3-minimal) > .bp3-popover-wrapper:not(:first-child) .bp3-button,
  .bp3-button-group:not(.bp3-minimal) > .bp3-button:not(:first-child){
    border-bottom-left-radius:0;
    border-top-left-radius:0; }
  .bp3-button-group:not(.bp3-minimal) > .bp3-popover-wrapper:not(:last-child) .bp3-button,
  .bp3-button-group:not(.bp3-minimal) > .bp3-button:not(:last-child){
    border-bottom-right-radius:0;
    border-top-right-radius:0;
    margin-right:-1px; }
  .bp3-button-group.bp3-minimal .bp3-button{
    background:none;
    -webkit-box-shadow:none;
            box-shadow:none; }
    .bp3-button-group.bp3-minimal .bp3-button:hover{
      background:rgba(167, 182, 194, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026;
      text-decoration:none; }
    .bp3-button-group.bp3-minimal .bp3-button:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-active{
      background:rgba(115, 134, 148, 0.3);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#182026; }
    .bp3-button-group.bp3-minimal .bp3-button:disabled, .bp3-button-group.bp3-minimal .bp3-button:disabled:hover, .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled, .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled:hover{
      background:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed; }
      .bp3-button-group.bp3-minimal .bp3-button:disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button:disabled:hover.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled:hover.bp3-active{
        background:rgba(115, 134, 148, 0.3); }
    .bp3-dark .bp3-button-group.bp3-minimal .bp3-button{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:inherit; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:hover, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:hover{
        background:rgba(138, 155, 168, 0.15); }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-active{
        background:rgba(138, 155, 168, 0.3);
        color:#f5f8fa; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:disabled:hover, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled:hover{
        background:none;
        color:rgba(167, 182, 194, 0.6);
        cursor:not-allowed; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button:disabled:hover.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-disabled:hover.bp3-active{
          background:rgba(138, 155, 168, 0.3); }
    .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary{
      color:#106ba3; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:hover, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#106ba3; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:hover{
        background:rgba(19, 124, 189, 0.15);
        color:#106ba3; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-active{
        background:rgba(19, 124, 189, 0.3);
        color:#106ba3; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:disabled, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-disabled{
        background:none;
        color:rgba(16, 107, 163, 0.5); }
        .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-disabled.bp3-active{
          background:rgba(19, 124, 189, 0.3); }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary .bp3-button-spinner .bp3-spinner-head{
        stroke:#106ba3; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary{
        color:#48aff0; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:hover{
          background:rgba(19, 124, 189, 0.2);
          color:#48aff0; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-active{
          background:rgba(19, 124, 189, 0.3);
          color:#48aff0; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-disabled{
          background:none;
          color:rgba(72, 175, 240, 0.5); }
          .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary:disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-primary.bp3-disabled.bp3-active{
            background:rgba(19, 124, 189, 0.3); }
    .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success{
      color:#0d8050; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:hover, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#0d8050; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:hover{
        background:rgba(15, 153, 96, 0.15);
        color:#0d8050; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-active{
        background:rgba(15, 153, 96, 0.3);
        color:#0d8050; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:disabled, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-disabled{
        background:none;
        color:rgba(13, 128, 80, 0.5); }
        .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-disabled.bp3-active{
          background:rgba(15, 153, 96, 0.3); }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success .bp3-button-spinner .bp3-spinner-head{
        stroke:#0d8050; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success{
        color:#3dcc91; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:hover{
          background:rgba(15, 153, 96, 0.2);
          color:#3dcc91; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-active{
          background:rgba(15, 153, 96, 0.3);
          color:#3dcc91; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-disabled{
          background:none;
          color:rgba(61, 204, 145, 0.5); }
          .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success:disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-success.bp3-disabled.bp3-active{
            background:rgba(15, 153, 96, 0.3); }
    .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning{
      color:#bf7326; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:hover, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#bf7326; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:hover{
        background:rgba(217, 130, 43, 0.15);
        color:#bf7326; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-active{
        background:rgba(217, 130, 43, 0.3);
        color:#bf7326; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:disabled, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-disabled{
        background:none;
        color:rgba(191, 115, 38, 0.5); }
        .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-disabled.bp3-active{
          background:rgba(217, 130, 43, 0.3); }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning .bp3-button-spinner .bp3-spinner-head{
        stroke:#bf7326; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning{
        color:#ffb366; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:hover{
          background:rgba(217, 130, 43, 0.2);
          color:#ffb366; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-active{
          background:rgba(217, 130, 43, 0.3);
          color:#ffb366; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-disabled{
          background:none;
          color:rgba(255, 179, 102, 0.5); }
          .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning:disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-warning.bp3-disabled.bp3-active{
            background:rgba(217, 130, 43, 0.3); }
    .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger{
      color:#c23030; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:hover, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-active{
        background:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:#c23030; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:hover{
        background:rgba(219, 55, 55, 0.15);
        color:#c23030; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-active{
        background:rgba(219, 55, 55, 0.3);
        color:#c23030; }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:disabled, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-disabled{
        background:none;
        color:rgba(194, 48, 48, 0.5); }
        .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:disabled.bp3-active, .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-disabled.bp3-active{
          background:rgba(219, 55, 55, 0.3); }
      .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger .bp3-button-spinner .bp3-spinner-head{
        stroke:#c23030; }
      .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger{
        color:#ff7373; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:hover{
          background:rgba(219, 55, 55, 0.2);
          color:#ff7373; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-active{
          background:rgba(219, 55, 55, 0.3);
          color:#ff7373; }
        .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:disabled, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-disabled{
          background:none;
          color:rgba(255, 115, 115, 0.5); }
          .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger:disabled.bp3-active, .bp3-dark .bp3-button-group.bp3-minimal .bp3-button.bp3-intent-danger.bp3-disabled.bp3-active{
            background:rgba(219, 55, 55, 0.3); }
  .bp3-button-group .bp3-popover-wrapper,
  .bp3-button-group .bp3-popover-target{
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto; }
  .bp3-button-group.bp3-fill{
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    width:100%; }
  .bp3-button-group .bp3-button.bp3-fill,
  .bp3-button-group.bp3-fill .bp3-button:not(.bp3-fixed){
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto; }
  .bp3-button-group.bp3-vertical{
    -webkit-box-align:stretch;
        -ms-flex-align:stretch;
            align-items:stretch;
    -webkit-box-orient:vertical;
    -webkit-box-direction:normal;
        -ms-flex-direction:column;
            flex-direction:column;
    vertical-align:top; }
    .bp3-button-group.bp3-vertical.bp3-fill{
      height:100%;
      width:unset; }
    .bp3-button-group.bp3-vertical .bp3-button{
      margin-right:0 !important;
      width:100%; }
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-popover-wrapper:first-child .bp3-button,
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-button:first-child{
      border-radius:3px 3px 0 0; }
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-popover-wrapper:last-child .bp3-button,
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-button:last-child{
      border-radius:0 0 3px 3px; }
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-popover-wrapper:not(:last-child) .bp3-button,
    .bp3-button-group.bp3-vertical:not(.bp3-minimal) > .bp3-button:not(:last-child){
      margin-bottom:-1px; }
  .bp3-button-group.bp3-align-left .bp3-button{
    text-align:left; }
  .bp3-dark .bp3-button-group:not(.bp3-minimal) > .bp3-popover-wrapper:not(:last-child) .bp3-button,
  .bp3-dark .bp3-button-group:not(.bp3-minimal) > .bp3-button:not(:last-child){
    margin-right:1px; }
  .bp3-dark .bp3-button-group.bp3-vertical > .bp3-popover-wrapper:not(:last-child) .bp3-button,
  .bp3-dark .bp3-button-group.bp3-vertical > .bp3-button:not(:last-child){
    margin-bottom:1px; }
.bp3-callout{
  font-size:14px;
  line-height:1.5;
  background-color:rgba(138, 155, 168, 0.15);
  border-radius:3px;
  padding:10px 12px 9px;
  position:relative;
  width:100%; }
  .bp3-callout[class*="bp3-icon-"]{
    padding-left:40px; }
    .bp3-callout[class*="bp3-icon-"]::before{
      font-family:"Icons20", sans-serif;
      font-size:20px;
      font-style:normal;
      font-weight:400;
      line-height:1;
      -moz-osx-font-smoothing:grayscale;
      -webkit-font-smoothing:antialiased;
      color:#5c7080;
      left:10px;
      position:absolute;
      top:10px; }
  .bp3-callout.bp3-callout-icon{
    padding-left:40px; }
    .bp3-callout.bp3-callout-icon > .bp3-icon:first-child{
      color:#5c7080;
      left:10px;
      position:absolute;
      top:10px; }
  .bp3-callout .bp3-heading{
    line-height:20px;
    margin-bottom:5px;
    margin-top:0; }
    .bp3-callout .bp3-heading:last-child{
      margin-bottom:0; }
  .bp3-dark .bp3-callout{
    background-color:rgba(138, 155, 168, 0.2); }
    .bp3-dark .bp3-callout[class*="bp3-icon-"]::before{
      color:#a7b6c2; }
  .bp3-callout.bp3-intent-primary{
    background-color:rgba(19, 124, 189, 0.15); }
    .bp3-callout.bp3-intent-primary[class*="bp3-icon-"]::before,
    .bp3-callout.bp3-intent-primary > .bp3-icon:first-child,
    .bp3-callout.bp3-intent-primary .bp3-heading{
      color:#106ba3; }
    .bp3-dark .bp3-callout.bp3-intent-primary{
      background-color:rgba(19, 124, 189, 0.25); }
      .bp3-dark .bp3-callout.bp3-intent-primary[class*="bp3-icon-"]::before,
      .bp3-dark .bp3-callout.bp3-intent-primary > .bp3-icon:first-child,
      .bp3-dark .bp3-callout.bp3-intent-primary .bp3-heading{
        color:#48aff0; }
  .bp3-callout.bp3-intent-success{
    background-color:rgba(15, 153, 96, 0.15); }
    .bp3-callout.bp3-intent-success[class*="bp3-icon-"]::before,
    .bp3-callout.bp3-intent-success > .bp3-icon:first-child,
    .bp3-callout.bp3-intent-success .bp3-heading{
      color:#0d8050; }
    .bp3-dark .bp3-callout.bp3-intent-success{
      background-color:rgba(15, 153, 96, 0.25); }
      .bp3-dark .bp3-callout.bp3-intent-success[class*="bp3-icon-"]::before,
      .bp3-dark .bp3-callout.bp3-intent-success > .bp3-icon:first-child,
      .bp3-dark .bp3-callout.bp3-intent-success .bp3-heading{
        color:#3dcc91; }
  .bp3-callout.bp3-intent-warning{
    background-color:rgba(217, 130, 43, 0.15); }
    .bp3-callout.bp3-intent-warning[class*="bp3-icon-"]::before,
    .bp3-callout.bp3-intent-warning > .bp3-icon:first-child,
    .bp3-callout.bp3-intent-warning .bp3-heading{
      color:#bf7326; }
    .bp3-dark .bp3-callout.bp3-intent-warning{
      background-color:rgba(217, 130, 43, 0.25); }
      .bp3-dark .bp3-callout.bp3-intent-warning[class*="bp3-icon-"]::before,
      .bp3-dark .bp3-callout.bp3-intent-warning > .bp3-icon:first-child,
      .bp3-dark .bp3-callout.bp3-intent-warning .bp3-heading{
        color:#ffb366; }
  .bp3-callout.bp3-intent-danger{
    background-color:rgba(219, 55, 55, 0.15); }
    .bp3-callout.bp3-intent-danger[class*="bp3-icon-"]::before,
    .bp3-callout.bp3-intent-danger > .bp3-icon:first-child,
    .bp3-callout.bp3-intent-danger .bp3-heading{
      color:#c23030; }
    .bp3-dark .bp3-callout.bp3-intent-danger{
      background-color:rgba(219, 55, 55, 0.25); }
      .bp3-dark .bp3-callout.bp3-intent-danger[class*="bp3-icon-"]::before,
      .bp3-dark .bp3-callout.bp3-intent-danger > .bp3-icon:first-child,
      .bp3-dark .bp3-callout.bp3-intent-danger .bp3-heading{
        color:#ff7373; }
  .bp3-running-text .bp3-callout{
    margin:20px 0; }
.bp3-card{
  background-color:#ffffff;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.15), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.15), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0);
  padding:20px;
  -webkit-transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), box-shadow 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), box-shadow 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 200ms cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-card.bp3-dark,
  .bp3-dark .bp3-card{
    background-color:#30404d;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0); }

.bp3-elevation-0{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.15), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.15), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0); }
  .bp3-elevation-0.bp3-dark,
  .bp3-dark .bp3-elevation-0{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), 0 0 0 rgba(16, 22, 26, 0), 0 0 0 rgba(16, 22, 26, 0); }

.bp3-elevation-1{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-elevation-1.bp3-dark,
  .bp3-dark .bp3-elevation-1{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4); }

.bp3-elevation-2{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 1px 1px rgba(16, 22, 26, 0.2), 0 2px 6px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 1px 1px rgba(16, 22, 26, 0.2), 0 2px 6px rgba(16, 22, 26, 0.2); }
  .bp3-elevation-2.bp3-dark,
  .bp3-dark .bp3-elevation-2{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.4), 0 2px 6px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.4), 0 2px 6px rgba(16, 22, 26, 0.4); }

.bp3-elevation-3{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2); }
  .bp3-elevation-3.bp3-dark,
  .bp3-dark .bp3-elevation-3{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }

.bp3-elevation-4{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2); }
  .bp3-elevation-4.bp3-dark,
  .bp3-dark .bp3-elevation-4{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4); }

.bp3-card.bp3-interactive:hover{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
  cursor:pointer; }
  .bp3-card.bp3-interactive:hover.bp3-dark,
  .bp3-dark .bp3-card.bp3-interactive:hover{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }

.bp3-card.bp3-interactive:active{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
  opacity:0.9;
  -webkit-transition-duration:0;
          transition-duration:0; }
  .bp3-card.bp3-interactive:active.bp3-dark,
  .bp3-dark .bp3-card.bp3-interactive:active{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4); }

.bp3-collapse{
  height:0;
  overflow-y:hidden;
  -webkit-transition:height 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:height 200ms cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-collapse .bp3-collapse-body{
    -webkit-transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-collapse .bp3-collapse-body[aria-hidden="true"]{
      display:none; }

.bp3-context-menu .bp3-popover-target{
  display:block; }

.bp3-context-menu-popover-target{
  position:fixed; }

.bp3-divider{
  border-bottom:1px solid rgba(16, 22, 26, 0.15);
  border-right:1px solid rgba(16, 22, 26, 0.15);
  margin:5px; }
  .bp3-dark .bp3-divider{
    border-color:rgba(16, 22, 26, 0.4); }
.bp3-dialog-container{
  opacity:1;
  -webkit-transform:scale(1);
          transform:scale(1);
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  min-height:100%;
  pointer-events:none;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none;
  width:100%; }
  .bp3-dialog-container.bp3-overlay-enter > .bp3-dialog, .bp3-dialog-container.bp3-overlay-appear > .bp3-dialog{
    opacity:0;
    -webkit-transform:scale(0.5);
            transform:scale(0.5); }
  .bp3-dialog-container.bp3-overlay-enter-active > .bp3-dialog, .bp3-dialog-container.bp3-overlay-appear-active > .bp3-dialog{
    opacity:1;
    -webkit-transform:scale(1);
            transform:scale(1);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:opacity, -webkit-transform;
    transition-property:opacity, -webkit-transform;
    transition-property:opacity, transform;
    transition-property:opacity, transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }
  .bp3-dialog-container.bp3-overlay-exit > .bp3-dialog{
    opacity:1;
    -webkit-transform:scale(1);
            transform:scale(1); }
  .bp3-dialog-container.bp3-overlay-exit-active > .bp3-dialog{
    opacity:0;
    -webkit-transform:scale(0.5);
            transform:scale(0.5);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:opacity, -webkit-transform;
    transition-property:opacity, -webkit-transform;
    transition-property:opacity, transform;
    transition-property:opacity, transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }

.bp3-dialog{
  background:#ebf1f5;
  border-radius:6px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  margin:30px 0;
  padding-bottom:20px;
  pointer-events:all;
  -webkit-user-select:text;
     -moz-user-select:text;
      -ms-user-select:text;
          user-select:text;
  width:500px; }
  .bp3-dialog:focus{
    outline:0; }
  .bp3-dialog.bp3-dark,
  .bp3-dark .bp3-dialog{
    background:#293742;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }

.bp3-dialog-header{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  background:#ffffff;
  border-radius:6px 6px 0 0;
  -webkit-box-shadow:0 1px 0 rgba(16, 22, 26, 0.15);
          box-shadow:0 1px 0 rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  min-height:40px;
  padding-left:20px;
  padding-right:5px;
  z-index:30; }
  .bp3-dialog-header .bp3-icon-large,
  .bp3-dialog-header .bp3-icon{
    color:#5c7080;
    -webkit-box-flex:0;
        -ms-flex:0 0 auto;
            flex:0 0 auto;
    margin-right:10px; }
  .bp3-dialog-header .bp3-heading{
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
    word-wrap:normal;
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto;
    line-height:inherit;
    margin:0; }
    .bp3-dialog-header .bp3-heading:last-child{
      margin-right:20px; }
  .bp3-dark .bp3-dialog-header{
    background:#30404d;
    -webkit-box-shadow:0 1px 0 rgba(16, 22, 26, 0.4);
            box-shadow:0 1px 0 rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-dialog-header .bp3-icon-large,
    .bp3-dark .bp3-dialog-header .bp3-icon{
      color:#a7b6c2; }

.bp3-dialog-body{
  -webkit-box-flex:1;
      -ms-flex:1 1 auto;
          flex:1 1 auto;
  line-height:18px;
  margin:20px; }

.bp3-dialog-footer{
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  margin:0 20px; }

.bp3-dialog-footer-actions{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-pack:end;
      -ms-flex-pack:end;
          justify-content:flex-end; }
  .bp3-dialog-footer-actions .bp3-button{
    margin-left:10px; }
.bp3-multistep-dialog-panels{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex; }

.bp3-multistep-dialog-left-panel{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-flex:1;
      -ms-flex:1;
          flex:1;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column; }
  .bp3-dark .bp3-multistep-dialog-left-panel{
    background:#202b33; }

.bp3-multistep-dialog-right-panel{
  background-color:#f5f8fa;
  border-left:1px solid rgba(16, 22, 26, 0.15);
  border-radius:0 0 6px 0;
  -webkit-box-flex:3;
      -ms-flex:3;
          flex:3;
  min-width:0; }
  .bp3-dark .bp3-multistep-dialog-right-panel{
    background-color:#293742;
    border-left:1px solid rgba(16, 22, 26, 0.4); }

.bp3-multistep-dialog-footer{
  background-color:#ffffff;
  border-radius:0 0 6px 0;
  border-top:1px solid rgba(16, 22, 26, 0.15);
  padding:10px; }
  .bp3-dark .bp3-multistep-dialog-footer{
    background:#30404d;
    border-top:1px solid rgba(16, 22, 26, 0.4); }

.bp3-dialog-step-container{
  background-color:#f5f8fa;
  border-bottom:1px solid rgba(16, 22, 26, 0.15); }
  .bp3-dark .bp3-dialog-step-container{
    background:#293742;
    border-bottom:1px solid rgba(16, 22, 26, 0.4); }
  .bp3-dialog-step-container.bp3-dialog-step-viewed{
    background-color:#ffffff; }
    .bp3-dark .bp3-dialog-step-container.bp3-dialog-step-viewed{
      background:#30404d; }

.bp3-dialog-step{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  background-color:#f5f8fa;
  border-radius:6px;
  cursor:not-allowed;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  margin:4px;
  padding:6px 14px; }
  .bp3-dark .bp3-dialog-step{
    background:#293742; }
  .bp3-dialog-step-viewed .bp3-dialog-step{
    background-color:#ffffff;
    cursor:pointer; }
    .bp3-dark .bp3-dialog-step-viewed .bp3-dialog-step{
      background:#30404d; }
  .bp3-dialog-step:hover{
    background-color:#f5f8fa; }
    .bp3-dark .bp3-dialog-step:hover{
      background:#293742; }

.bp3-dialog-step-icon{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  background-color:rgba(92, 112, 128, 0.6);
  border-radius:50%;
  color:#ffffff;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  height:25px;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  width:25px; }
  .bp3-dark .bp3-dialog-step-icon{
    background-color:rgba(167, 182, 194, 0.6); }
  .bp3-active.bp3-dialog-step-viewed .bp3-dialog-step-icon{
    background-color:#2b95d6; }
  .bp3-dialog-step-viewed .bp3-dialog-step-icon{
    background-color:#8a9ba8; }

.bp3-dialog-step-title{
  color:rgba(92, 112, 128, 0.6);
  -webkit-box-flex:1;
      -ms-flex:1;
          flex:1;
  padding-left:10px; }
  .bp3-dark .bp3-dialog-step-title{
    color:rgba(167, 182, 194, 0.6); }
  .bp3-active.bp3-dialog-step-viewed .bp3-dialog-step-title{
    color:#2b95d6; }
  .bp3-dialog-step-viewed:not(.bp3-active) .bp3-dialog-step-title{
    color:#182026; }
    .bp3-dark .bp3-dialog-step-viewed:not(.bp3-active) .bp3-dialog-step-title{
      color:#f5f8fa; }
.bp3-drawer{
  background:#ffffff;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  margin:0;
  padding:0; }
  .bp3-drawer:focus{
    outline:0; }
  .bp3-drawer.bp3-position-top{
    height:50%;
    left:0;
    right:0;
    top:0; }
    .bp3-drawer.bp3-position-top.bp3-overlay-enter, .bp3-drawer.bp3-position-top.bp3-overlay-appear{
      -webkit-transform:translateY(-100%);
              transform:translateY(-100%); }
    .bp3-drawer.bp3-position-top.bp3-overlay-enter-active, .bp3-drawer.bp3-position-top.bp3-overlay-appear-active{
      -webkit-transform:translateY(0);
              transform:translateY(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer.bp3-position-top.bp3-overlay-exit{
      -webkit-transform:translateY(0);
              transform:translateY(0); }
    .bp3-drawer.bp3-position-top.bp3-overlay-exit-active{
      -webkit-transform:translateY(-100%);
              transform:translateY(-100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer.bp3-position-bottom{
    bottom:0;
    height:50%;
    left:0;
    right:0; }
    .bp3-drawer.bp3-position-bottom.bp3-overlay-enter, .bp3-drawer.bp3-position-bottom.bp3-overlay-appear{
      -webkit-transform:translateY(100%);
              transform:translateY(100%); }
    .bp3-drawer.bp3-position-bottom.bp3-overlay-enter-active, .bp3-drawer.bp3-position-bottom.bp3-overlay-appear-active{
      -webkit-transform:translateY(0);
              transform:translateY(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer.bp3-position-bottom.bp3-overlay-exit{
      -webkit-transform:translateY(0);
              transform:translateY(0); }
    .bp3-drawer.bp3-position-bottom.bp3-overlay-exit-active{
      -webkit-transform:translateY(100%);
              transform:translateY(100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer.bp3-position-left{
    bottom:0;
    left:0;
    top:0;
    width:50%; }
    .bp3-drawer.bp3-position-left.bp3-overlay-enter, .bp3-drawer.bp3-position-left.bp3-overlay-appear{
      -webkit-transform:translateX(-100%);
              transform:translateX(-100%); }
    .bp3-drawer.bp3-position-left.bp3-overlay-enter-active, .bp3-drawer.bp3-position-left.bp3-overlay-appear-active{
      -webkit-transform:translateX(0);
              transform:translateX(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer.bp3-position-left.bp3-overlay-exit{
      -webkit-transform:translateX(0);
              transform:translateX(0); }
    .bp3-drawer.bp3-position-left.bp3-overlay-exit-active{
      -webkit-transform:translateX(-100%);
              transform:translateX(-100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer.bp3-position-right{
    bottom:0;
    right:0;
    top:0;
    width:50%; }
    .bp3-drawer.bp3-position-right.bp3-overlay-enter, .bp3-drawer.bp3-position-right.bp3-overlay-appear{
      -webkit-transform:translateX(100%);
              transform:translateX(100%); }
    .bp3-drawer.bp3-position-right.bp3-overlay-enter-active, .bp3-drawer.bp3-position-right.bp3-overlay-appear-active{
      -webkit-transform:translateX(0);
              transform:translateX(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer.bp3-position-right.bp3-overlay-exit{
      -webkit-transform:translateX(0);
              transform:translateX(0); }
    .bp3-drawer.bp3-position-right.bp3-overlay-exit-active{
      -webkit-transform:translateX(100%);
              transform:translateX(100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
  .bp3-position-right):not(.bp3-vertical){
    bottom:0;
    right:0;
    top:0;
    width:50%; }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-enter, .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-appear{
      -webkit-transform:translateX(100%);
              transform:translateX(100%); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-enter-active, .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-appear-active{
      -webkit-transform:translateX(0);
              transform:translateX(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-exit{
      -webkit-transform:translateX(0);
              transform:translateX(0); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right):not(.bp3-vertical).bp3-overlay-exit-active{
      -webkit-transform:translateX(100%);
              transform:translateX(100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
  .bp3-position-right).bp3-vertical{
    bottom:0;
    height:50%;
    left:0;
    right:0; }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-enter, .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-appear{
      -webkit-transform:translateY(100%);
              transform:translateY(100%); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-enter-active, .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-appear-active{
      -webkit-transform:translateY(0);
              transform:translateY(0);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:200ms;
              transition-duration:200ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-exit{
      -webkit-transform:translateY(0);
              transform:translateY(0); }
    .bp3-drawer:not(.bp3-position-top):not(.bp3-position-bottom):not(.bp3-position-left):not(
    .bp3-position-right).bp3-vertical.bp3-overlay-exit-active{
      -webkit-transform:translateY(100%);
              transform:translateY(100%);
      -webkit-transition-delay:0;
              transition-delay:0;
      -webkit-transition-duration:100ms;
              transition-duration:100ms;
      -webkit-transition-property:-webkit-transform;
      transition-property:-webkit-transform;
      transition-property:transform;
      transition-property:transform, -webkit-transform;
      -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
              transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-drawer.bp3-dark,
  .bp3-dark .bp3-drawer{
    background:#30404d;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }

.bp3-drawer-header{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  border-radius:0;
  -webkit-box-shadow:0 1px 0 rgba(16, 22, 26, 0.15);
          box-shadow:0 1px 0 rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  min-height:40px;
  padding:5px;
  padding-left:20px;
  position:relative; }
  .bp3-drawer-header .bp3-icon-large,
  .bp3-drawer-header .bp3-icon{
    color:#5c7080;
    -webkit-box-flex:0;
        -ms-flex:0 0 auto;
            flex:0 0 auto;
    margin-right:10px; }
  .bp3-drawer-header .bp3-heading{
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
    word-wrap:normal;
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto;
    line-height:inherit;
    margin:0; }
    .bp3-drawer-header .bp3-heading:last-child{
      margin-right:20px; }
  .bp3-dark .bp3-drawer-header{
    -webkit-box-shadow:0 1px 0 rgba(16, 22, 26, 0.4);
            box-shadow:0 1px 0 rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-drawer-header .bp3-icon-large,
    .bp3-dark .bp3-drawer-header .bp3-icon{
      color:#a7b6c2; }

.bp3-drawer-body{
  -webkit-box-flex:1;
      -ms-flex:1 1 auto;
          flex:1 1 auto;
  line-height:18px;
  overflow:auto; }

.bp3-drawer-footer{
  -webkit-box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.15);
          box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.15);
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  padding:10px 20px;
  position:relative; }
  .bp3-dark .bp3-drawer-footer{
    -webkit-box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.4);
            box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.4); }
.bp3-editable-text{
  cursor:text;
  display:inline-block;
  max-width:100%;
  position:relative;
  vertical-align:top;
  white-space:nowrap; }
  .bp3-editable-text::before{
    bottom:-3px;
    left:-3px;
    position:absolute;
    right:-3px;
    top:-3px;
    border-radius:3px;
    content:"";
    -webkit-transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9), box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9), box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-editable-text:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15); }
  .bp3-editable-text.bp3-editable-text-editing::before{
    background-color:#ffffff;
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-editable-text.bp3-disabled::before{
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-editable-text.bp3-intent-primary .bp3-editable-text-input,
  .bp3-editable-text.bp3-intent-primary .bp3-editable-text-content{
    color:#137cbd; }
  .bp3-editable-text.bp3-intent-primary:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(19, 124, 189, 0.4);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(19, 124, 189, 0.4); }
  .bp3-editable-text.bp3-intent-primary.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-editable-text.bp3-intent-success .bp3-editable-text-input,
  .bp3-editable-text.bp3-intent-success .bp3-editable-text-content{
    color:#0f9960; }
  .bp3-editable-text.bp3-intent-success:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px rgba(15, 153, 96, 0.4);
            box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px rgba(15, 153, 96, 0.4); }
  .bp3-editable-text.bp3-intent-success.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-editable-text.bp3-intent-warning .bp3-editable-text-input,
  .bp3-editable-text.bp3-intent-warning .bp3-editable-text-content{
    color:#d9822b; }
  .bp3-editable-text.bp3-intent-warning:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px rgba(217, 130, 43, 0.4);
            box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px rgba(217, 130, 43, 0.4); }
  .bp3-editable-text.bp3-intent-warning.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-editable-text.bp3-intent-danger .bp3-editable-text-input,
  .bp3-editable-text.bp3-intent-danger .bp3-editable-text-content{
    color:#db3737; }
  .bp3-editable-text.bp3-intent-danger:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px rgba(219, 55, 55, 0.4);
            box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px rgba(219, 55, 55, 0.4); }
  .bp3-editable-text.bp3-intent-danger.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-editable-text:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(255, 255, 255, 0.15);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(255, 255, 255, 0.15); }
  .bp3-dark .bp3-editable-text.bp3-editable-text-editing::before{
    background-color:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-disabled::before{
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-dark .bp3-editable-text.bp3-intent-primary .bp3-editable-text-content{
    color:#48aff0; }
  .bp3-dark .bp3-editable-text.bp3-intent-primary:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(72, 175, 240, 0), 0 0 0 0 rgba(72, 175, 240, 0), inset 0 0 0 1px rgba(72, 175, 240, 0.4);
            box-shadow:0 0 0 0 rgba(72, 175, 240, 0), 0 0 0 0 rgba(72, 175, 240, 0), inset 0 0 0 1px rgba(72, 175, 240, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-primary.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #48aff0, 0 0 0 3px rgba(72, 175, 240, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #48aff0, 0 0 0 3px rgba(72, 175, 240, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-success .bp3-editable-text-content{
    color:#3dcc91; }
  .bp3-dark .bp3-editable-text.bp3-intent-success:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(61, 204, 145, 0), 0 0 0 0 rgba(61, 204, 145, 0), inset 0 0 0 1px rgba(61, 204, 145, 0.4);
            box-shadow:0 0 0 0 rgba(61, 204, 145, 0), 0 0 0 0 rgba(61, 204, 145, 0), inset 0 0 0 1px rgba(61, 204, 145, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-success.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #3dcc91, 0 0 0 3px rgba(61, 204, 145, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #3dcc91, 0 0 0 3px rgba(61, 204, 145, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-warning .bp3-editable-text-content{
    color:#ffb366; }
  .bp3-dark .bp3-editable-text.bp3-intent-warning:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(255, 179, 102, 0), 0 0 0 0 rgba(255, 179, 102, 0), inset 0 0 0 1px rgba(255, 179, 102, 0.4);
            box-shadow:0 0 0 0 rgba(255, 179, 102, 0), 0 0 0 0 rgba(255, 179, 102, 0), inset 0 0 0 1px rgba(255, 179, 102, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-warning.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #ffb366, 0 0 0 3px rgba(255, 179, 102, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #ffb366, 0 0 0 3px rgba(255, 179, 102, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-danger .bp3-editable-text-content{
    color:#ff7373; }
  .bp3-dark .bp3-editable-text.bp3-intent-danger:hover::before{
    -webkit-box-shadow:0 0 0 0 rgba(255, 115, 115, 0), 0 0 0 0 rgba(255, 115, 115, 0), inset 0 0 0 1px rgba(255, 115, 115, 0.4);
            box-shadow:0 0 0 0 rgba(255, 115, 115, 0), 0 0 0 0 rgba(255, 115, 115, 0), inset 0 0 0 1px rgba(255, 115, 115, 0.4); }
  .bp3-dark .bp3-editable-text.bp3-intent-danger.bp3-editable-text-editing::before{
    -webkit-box-shadow:0 0 0 1px #ff7373, 0 0 0 3px rgba(255, 115, 115, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #ff7373, 0 0 0 3px rgba(255, 115, 115, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }

.bp3-editable-text-input,
.bp3-editable-text-content{
  color:inherit;
  display:inherit;
  font:inherit;
  letter-spacing:inherit;
  max-width:inherit;
  min-width:inherit;
  position:relative;
  resize:none;
  text-transform:inherit;
  vertical-align:top; }

.bp3-editable-text-input{
  background:none;
  border:none;
  -webkit-box-shadow:none;
          box-shadow:none;
  padding:0;
  white-space:pre-wrap;
  width:100%; }
  .bp3-editable-text-input::-webkit-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-editable-text-input::-moz-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-editable-text-input:-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-editable-text-input::-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-editable-text-input::placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-editable-text-input:focus{
    outline:none; }
  .bp3-editable-text-input::-ms-clear{
    display:none; }

.bp3-editable-text-content{
  overflow:hidden;
  padding-right:2px;
  text-overflow:ellipsis;
  white-space:pre; }
  .bp3-editable-text-editing > .bp3-editable-text-content{
    left:0;
    position:absolute;
    visibility:hidden; }
  .bp3-editable-text-placeholder > .bp3-editable-text-content{
    color:rgba(92, 112, 128, 0.6); }
    .bp3-dark .bp3-editable-text-placeholder > .bp3-editable-text-content{
      color:rgba(167, 182, 194, 0.6); }

.bp3-editable-text.bp3-multiline{
  display:block; }
  .bp3-editable-text.bp3-multiline .bp3-editable-text-content{
    overflow:auto;
    white-space:pre-wrap;
    word-wrap:break-word; }
.bp3-divider{
  border-bottom:1px solid rgba(16, 22, 26, 0.15);
  border-right:1px solid rgba(16, 22, 26, 0.15);
  margin:5px; }
  .bp3-dark .bp3-divider{
    border-color:rgba(16, 22, 26, 0.4); }
.bp3-control-group{
  -webkit-transform:translateZ(0);
          transform:translateZ(0);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:stretch;
      -ms-flex-align:stretch;
          align-items:stretch; }
  .bp3-control-group > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-control-group > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-control-group .bp3-button,
  .bp3-control-group .bp3-html-select,
  .bp3-control-group .bp3-input,
  .bp3-control-group .bp3-select{
    position:relative; }
  .bp3-control-group .bp3-input{
    border-radius:inherit;
    z-index:2; }
    .bp3-control-group .bp3-input:focus{
      border-radius:3px;
      z-index:14; }
    .bp3-control-group .bp3-input[class*="bp3-intent"]{
      z-index:13; }
      .bp3-control-group .bp3-input[class*="bp3-intent"]:focus{
        z-index:15; }
    .bp3-control-group .bp3-input[readonly], .bp3-control-group .bp3-input:disabled, .bp3-control-group .bp3-input.bp3-disabled{
      z-index:1; }
  .bp3-control-group .bp3-input-group[class*="bp3-intent"] .bp3-input{
    z-index:13; }
    .bp3-control-group .bp3-input-group[class*="bp3-intent"] .bp3-input:focus{
      z-index:15; }
  .bp3-control-group .bp3-button,
  .bp3-control-group .bp3-html-select select,
  .bp3-control-group .bp3-select select{
    -webkit-transform:translateZ(0);
            transform:translateZ(0);
    border-radius:inherit;
    z-index:4; }
    .bp3-control-group .bp3-button:focus,
    .bp3-control-group .bp3-html-select select:focus,
    .bp3-control-group .bp3-select select:focus{
      z-index:5; }
    .bp3-control-group .bp3-button:hover,
    .bp3-control-group .bp3-html-select select:hover,
    .bp3-control-group .bp3-select select:hover{
      z-index:6; }
    .bp3-control-group .bp3-button:active,
    .bp3-control-group .bp3-html-select select:active,
    .bp3-control-group .bp3-select select:active{
      z-index:7; }
    .bp3-control-group .bp3-button[readonly], .bp3-control-group .bp3-button:disabled, .bp3-control-group .bp3-button.bp3-disabled,
    .bp3-control-group .bp3-html-select select[readonly],
    .bp3-control-group .bp3-html-select select:disabled,
    .bp3-control-group .bp3-html-select select.bp3-disabled,
    .bp3-control-group .bp3-select select[readonly],
    .bp3-control-group .bp3-select select:disabled,
    .bp3-control-group .bp3-select select.bp3-disabled{
      z-index:3; }
    .bp3-control-group .bp3-button[class*="bp3-intent"],
    .bp3-control-group .bp3-html-select select[class*="bp3-intent"],
    .bp3-control-group .bp3-select select[class*="bp3-intent"]{
      z-index:9; }
      .bp3-control-group .bp3-button[class*="bp3-intent"]:focus,
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"]:focus,
      .bp3-control-group .bp3-select select[class*="bp3-intent"]:focus{
        z-index:10; }
      .bp3-control-group .bp3-button[class*="bp3-intent"]:hover,
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"]:hover,
      .bp3-control-group .bp3-select select[class*="bp3-intent"]:hover{
        z-index:11; }
      .bp3-control-group .bp3-button[class*="bp3-intent"]:active,
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"]:active,
      .bp3-control-group .bp3-select select[class*="bp3-intent"]:active{
        z-index:12; }
      .bp3-control-group .bp3-button[class*="bp3-intent"][readonly], .bp3-control-group .bp3-button[class*="bp3-intent"]:disabled, .bp3-control-group .bp3-button[class*="bp3-intent"].bp3-disabled,
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"][readonly],
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"]:disabled,
      .bp3-control-group .bp3-html-select select[class*="bp3-intent"].bp3-disabled,
      .bp3-control-group .bp3-select select[class*="bp3-intent"][readonly],
      .bp3-control-group .bp3-select select[class*="bp3-intent"]:disabled,
      .bp3-control-group .bp3-select select[class*="bp3-intent"].bp3-disabled{
        z-index:8; }
  .bp3-control-group .bp3-input-group > .bp3-icon,
  .bp3-control-group .bp3-input-group > .bp3-button,
  .bp3-control-group .bp3-input-group > .bp3-input-left-container,
  .bp3-control-group .bp3-input-group > .bp3-input-action{
    z-index:16; }
  .bp3-control-group .bp3-select::after,
  .bp3-control-group .bp3-html-select::after,
  .bp3-control-group .bp3-select > .bp3-icon,
  .bp3-control-group .bp3-html-select > .bp3-icon{
    z-index:17; }
  .bp3-control-group .bp3-select:focus-within{
    z-index:5; }
  .bp3-control-group:not(.bp3-vertical) > *:not(.bp3-divider){
    margin-right:-1px; }
  .bp3-control-group:not(.bp3-vertical) > .bp3-divider:not(:first-child){
    margin-left:6px; }
  .bp3-dark .bp3-control-group:not(.bp3-vertical) > *:not(.bp3-divider){
    margin-right:0; }
  .bp3-dark .bp3-control-group:not(.bp3-vertical) > .bp3-button + .bp3-button{
    margin-left:1px; }
  .bp3-control-group .bp3-popover-wrapper,
  .bp3-control-group .bp3-popover-target{
    border-radius:inherit; }
  .bp3-control-group > :first-child{
    border-radius:3px 0 0 3px; }
  .bp3-control-group > :last-child{
    border-radius:0 3px 3px 0;
    margin-right:0; }
  .bp3-control-group > :only-child{
    border-radius:3px;
    margin-right:0; }
  .bp3-control-group .bp3-input-group .bp3-button{
    border-radius:3px; }
  .bp3-control-group .bp3-numeric-input:not(:first-child) .bp3-input-group{
    border-bottom-left-radius:0;
    border-top-left-radius:0; }
  .bp3-control-group.bp3-fill{
    width:100%; }
  .bp3-control-group > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto; }
  .bp3-control-group.bp3-fill > *:not(.bp3-fixed){
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto; }
  .bp3-control-group.bp3-vertical{
    -webkit-box-orient:vertical;
    -webkit-box-direction:normal;
        -ms-flex-direction:column;
            flex-direction:column; }
    .bp3-control-group.bp3-vertical > *{
      margin-top:-1px; }
    .bp3-control-group.bp3-vertical > :first-child{
      border-radius:3px 3px 0 0;
      margin-top:0; }
    .bp3-control-group.bp3-vertical > :last-child{
      border-radius:0 0 3px 3px; }
.bp3-control{
  cursor:pointer;
  display:block;
  margin-bottom:10px;
  position:relative;
  text-transform:none; }
  .bp3-control input:checked ~ .bp3-control-indicator{
    background-color:#137cbd;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
  .bp3-control:hover input:checked ~ .bp3-control-indicator{
    background-color:#106ba3;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
  .bp3-control input:not(:disabled):active:checked ~ .bp3-control-indicator{
    background:#0e5a8a;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-control input:disabled:checked ~ .bp3-control-indicator{
    background:rgba(19, 124, 189, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-dark .bp3-control input:checked ~ .bp3-control-indicator{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control:hover input:checked ~ .bp3-control-indicator{
    background-color:#106ba3;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control input:not(:disabled):active:checked ~ .bp3-control-indicator{
    background-color:#0e5a8a;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-control input:disabled:checked ~ .bp3-control-indicator{
    background:rgba(14, 90, 138, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-control:not(.bp3-align-right){
    padding-left:26px; }
    .bp3-control:not(.bp3-align-right) .bp3-control-indicator{
      margin-left:-26px; }
  .bp3-control.bp3-align-right{
    padding-right:26px; }
    .bp3-control.bp3-align-right .bp3-control-indicator{
      margin-right:-26px; }
  .bp3-control.bp3-disabled{
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed; }
  .bp3-control.bp3-inline{
    display:inline-block;
    margin-right:20px; }
  .bp3-control input{
    left:0;
    opacity:0;
    position:absolute;
    top:0;
    z-index:-1; }
  .bp3-control .bp3-control-indicator{
    background-clip:padding-box;
    background-color:#f5f8fa;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.8)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0));
    border:none;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
    cursor:pointer;
    display:inline-block;
    font-size:16px;
    height:1em;
    margin-right:10px;
    margin-top:-3px;
    position:relative;
    -webkit-user-select:none;
       -moz-user-select:none;
        -ms-user-select:none;
            user-select:none;
    vertical-align:middle;
    width:1em; }
    .bp3-control .bp3-control-indicator::before{
      content:"";
      display:block;
      height:1em;
      width:1em; }
  .bp3-control:hover .bp3-control-indicator{
    background-color:#ebf1f5; }
  .bp3-control input:not(:disabled):active ~ .bp3-control-indicator{
    background:#d8e1e8;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-control input:disabled ~ .bp3-control-indicator{
    background:rgba(206, 217, 224, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none;
    cursor:not-allowed; }
  .bp3-control input:focus ~ .bp3-control-indicator{
    outline:rgba(19, 124, 189, 0.6) auto 2px;
    outline-offset:2px;
    -moz-outline-radius:6px; }
  .bp3-control.bp3-align-right .bp3-control-indicator{
    float:right;
    margin-left:10px;
    margin-top:1px; }
  .bp3-control.bp3-large{
    font-size:16px; }
    .bp3-control.bp3-large:not(.bp3-align-right){
      padding-left:30px; }
      .bp3-control.bp3-large:not(.bp3-align-right) .bp3-control-indicator{
        margin-left:-30px; }
    .bp3-control.bp3-large.bp3-align-right{
      padding-right:30px; }
      .bp3-control.bp3-large.bp3-align-right .bp3-control-indicator{
        margin-right:-30px; }
    .bp3-control.bp3-large .bp3-control-indicator{
      font-size:20px; }
    .bp3-control.bp3-large.bp3-align-right .bp3-control-indicator{
      margin-top:0; }
  .bp3-control.bp3-checkbox input:indeterminate ~ .bp3-control-indicator{
    background-color:#137cbd;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.1)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
    color:#ffffff; }
  .bp3-control.bp3-checkbox:hover input:indeterminate ~ .bp3-control-indicator{
    background-color:#106ba3;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 -1px 0 rgba(16, 22, 26, 0.2); }
  .bp3-control.bp3-checkbox input:not(:disabled):active:indeterminate ~ .bp3-control-indicator{
    background:#0e5a8a;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-control.bp3-checkbox input:disabled:indeterminate ~ .bp3-control-indicator{
    background:rgba(19, 124, 189, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-dark .bp3-control.bp3-checkbox input:indeterminate ~ .bp3-control-indicator{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control.bp3-checkbox:hover input:indeterminate ~ .bp3-control-indicator{
    background-color:#106ba3;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control.bp3-checkbox input:not(:disabled):active:indeterminate ~ .bp3-control-indicator{
    background-color:#0e5a8a;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-control.bp3-checkbox input:disabled:indeterminate ~ .bp3-control-indicator{
    background:rgba(14, 90, 138, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-control.bp3-checkbox .bp3-control-indicator{
    border-radius:3px; }
  .bp3-control.bp3-checkbox input:checked ~ .bp3-control-indicator::before{
    background-image:url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3e%3cpath fill-rule='evenodd' clip-rule='evenodd' d='M12 5c-.28 0-.53.11-.71.29L7 9.59l-2.29-2.3a1.003 1.003 0 00-1.42 1.42l3 3c.18.18.43.29.71.29s.53-.11.71-.29l5-5A1.003 1.003 0 0012 5z' fill='white'/%3e%3c/svg%3e"); }
  .bp3-control.bp3-checkbox input:indeterminate ~ .bp3-control-indicator::before{
    background-image:url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3e%3cpath fill-rule='evenodd' clip-rule='evenodd' d='M11 7H5c-.55 0-1 .45-1 1s.45 1 1 1h6c.55 0 1-.45 1-1s-.45-1-1-1z' fill='white'/%3e%3c/svg%3e"); }
  .bp3-control.bp3-radio .bp3-control-indicator{
    border-radius:50%; }
  .bp3-control.bp3-radio input:checked ~ .bp3-control-indicator::before{
    background-image:radial-gradient(#ffffff, #ffffff 28%, transparent 32%); }
  .bp3-control.bp3-radio input:checked:disabled ~ .bp3-control-indicator::before{
    opacity:0.5; }
  .bp3-control.bp3-radio input:focus ~ .bp3-control-indicator{
    -moz-outline-radius:16px; }
  .bp3-control.bp3-switch input ~ .bp3-control-indicator{
    background:rgba(167, 182, 194, 0.5); }
  .bp3-control.bp3-switch:hover input ~ .bp3-control-indicator{
    background:rgba(115, 134, 148, 0.5); }
  .bp3-control.bp3-switch input:not(:disabled):active ~ .bp3-control-indicator{
    background:rgba(92, 112, 128, 0.5); }
  .bp3-control.bp3-switch input:disabled ~ .bp3-control-indicator{
    background:rgba(206, 217, 224, 0.5); }
    .bp3-control.bp3-switch input:disabled ~ .bp3-control-indicator::before{
      background:rgba(255, 255, 255, 0.8); }
  .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator{
    background:#137cbd; }
  .bp3-control.bp3-switch:hover input:checked ~ .bp3-control-indicator{
    background:#106ba3; }
  .bp3-control.bp3-switch input:checked:not(:disabled):active ~ .bp3-control-indicator{
    background:#0e5a8a; }
  .bp3-control.bp3-switch input:checked:disabled ~ .bp3-control-indicator{
    background:rgba(19, 124, 189, 0.5); }
    .bp3-control.bp3-switch input:checked:disabled ~ .bp3-control-indicator::before{
      background:rgba(255, 255, 255, 0.8); }
  .bp3-control.bp3-switch:not(.bp3-align-right){
    padding-left:38px; }
    .bp3-control.bp3-switch:not(.bp3-align-right) .bp3-control-indicator{
      margin-left:-38px; }
  .bp3-control.bp3-switch.bp3-align-right{
    padding-right:38px; }
    .bp3-control.bp3-switch.bp3-align-right .bp3-control-indicator{
      margin-right:-38px; }
  .bp3-control.bp3-switch .bp3-control-indicator{
    border:none;
    border-radius:1.75em;
    -webkit-box-shadow:none !important;
            box-shadow:none !important;
    min-width:1.75em;
    -webkit-transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:background-color 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
    width:auto; }
    .bp3-control.bp3-switch .bp3-control-indicator::before{
      background:#ffffff;
      border-radius:50%;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
      height:calc(1em - 4px);
      left:0;
      margin:2px;
      position:absolute;
      -webkit-transition:left 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
      transition:left 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
      width:calc(1em - 4px); }
  .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator::before{
    left:calc(100% - 1em); }
  .bp3-control.bp3-switch.bp3-large:not(.bp3-align-right){
    padding-left:45px; }
    .bp3-control.bp3-switch.bp3-large:not(.bp3-align-right) .bp3-control-indicator{
      margin-left:-45px; }
  .bp3-control.bp3-switch.bp3-large.bp3-align-right{
    padding-right:45px; }
    .bp3-control.bp3-switch.bp3-large.bp3-align-right .bp3-control-indicator{
      margin-right:-45px; }
  .bp3-dark .bp3-control.bp3-switch input ~ .bp3-control-indicator{
    background:rgba(16, 22, 26, 0.5); }
  .bp3-dark .bp3-control.bp3-switch:hover input ~ .bp3-control-indicator{
    background:rgba(16, 22, 26, 0.7); }
  .bp3-dark .bp3-control.bp3-switch input:not(:disabled):active ~ .bp3-control-indicator{
    background:rgba(16, 22, 26, 0.9); }
  .bp3-dark .bp3-control.bp3-switch input:disabled ~ .bp3-control-indicator{
    background:rgba(57, 75, 89, 0.5); }
    .bp3-dark .bp3-control.bp3-switch input:disabled ~ .bp3-control-indicator::before{
      background:rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator{
    background:#137cbd; }
  .bp3-dark .bp3-control.bp3-switch:hover input:checked ~ .bp3-control-indicator{
    background:#106ba3; }
  .bp3-dark .bp3-control.bp3-switch input:checked:not(:disabled):active ~ .bp3-control-indicator{
    background:#0e5a8a; }
  .bp3-dark .bp3-control.bp3-switch input:checked:disabled ~ .bp3-control-indicator{
    background:rgba(14, 90, 138, 0.5); }
    .bp3-dark .bp3-control.bp3-switch input:checked:disabled ~ .bp3-control-indicator::before{
      background:rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control.bp3-switch .bp3-control-indicator::before{
    background:#394b59;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator::before{
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-control.bp3-switch .bp3-switch-inner-text{
    font-size:0.7em;
    text-align:center; }
  .bp3-control.bp3-switch .bp3-control-indicator-child:first-child{
    line-height:0;
    margin-left:0.5em;
    margin-right:1.2em;
    visibility:hidden; }
  .bp3-control.bp3-switch .bp3-control-indicator-child:last-child{
    line-height:1em;
    margin-left:1.2em;
    margin-right:0.5em;
    visibility:visible; }
  .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator .bp3-control-indicator-child:first-child{
    line-height:1em;
    visibility:visible; }
  .bp3-control.bp3-switch input:checked ~ .bp3-control-indicator .bp3-control-indicator-child:last-child{
    line-height:0;
    visibility:hidden; }
  .bp3-dark .bp3-control{
    color:#f5f8fa; }
    .bp3-dark .bp3-control.bp3-disabled{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-control .bp3-control-indicator{
      background-color:#394b59;
      background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.05)), to(rgba(255, 255, 255, 0)));
      background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-control:hover .bp3-control-indicator{
      background-color:#30404d; }
    .bp3-dark .bp3-control input:not(:disabled):active ~ .bp3-control-indicator{
      background:#202b33;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-dark .bp3-control input:disabled ~ .bp3-control-indicator{
      background:rgba(57, 75, 89, 0.5);
      -webkit-box-shadow:none;
              box-shadow:none;
      cursor:not-allowed; }
    .bp3-dark .bp3-control.bp3-checkbox input:disabled:checked ~ .bp3-control-indicator, .bp3-dark .bp3-control.bp3-checkbox input:disabled:indeterminate ~ .bp3-control-indicator{
      color:rgba(167, 182, 194, 0.6); }
.bp3-file-input{
  cursor:pointer;
  display:inline-block;
  height:30px;
  position:relative; }
  .bp3-file-input input{
    margin:0;
    min-width:200px;
    opacity:0; }
    .bp3-file-input input:disabled + .bp3-file-upload-input,
    .bp3-file-input input.bp3-disabled + .bp3-file-upload-input{
      background:rgba(206, 217, 224, 0.5);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed;
      resize:none; }
      .bp3-file-input input:disabled + .bp3-file-upload-input::after,
      .bp3-file-input input.bp3-disabled + .bp3-file-upload-input::after{
        background-color:rgba(206, 217, 224, 0.5);
        background-image:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:rgba(92, 112, 128, 0.6);
        cursor:not-allowed;
        outline:none; }
        .bp3-file-input input:disabled + .bp3-file-upload-input::after.bp3-active, .bp3-file-input input:disabled + .bp3-file-upload-input::after.bp3-active:hover,
        .bp3-file-input input.bp3-disabled + .bp3-file-upload-input::after.bp3-active,
        .bp3-file-input input.bp3-disabled + .bp3-file-upload-input::after.bp3-active:hover{
          background:rgba(206, 217, 224, 0.7); }
      .bp3-dark .bp3-file-input input:disabled + .bp3-file-upload-input, .bp3-dark
      .bp3-file-input input.bp3-disabled + .bp3-file-upload-input{
        background:rgba(57, 75, 89, 0.5);
        -webkit-box-shadow:none;
                box-shadow:none;
        color:rgba(167, 182, 194, 0.6); }
        .bp3-dark .bp3-file-input input:disabled + .bp3-file-upload-input::after, .bp3-dark
        .bp3-file-input input.bp3-disabled + .bp3-file-upload-input::after{
          background-color:rgba(57, 75, 89, 0.5);
          background-image:none;
          -webkit-box-shadow:none;
                  box-shadow:none;
          color:rgba(167, 182, 194, 0.6); }
          .bp3-dark .bp3-file-input input:disabled + .bp3-file-upload-input::after.bp3-active, .bp3-dark
          .bp3-file-input input.bp3-disabled + .bp3-file-upload-input::after.bp3-active{
            background:rgba(57, 75, 89, 0.7); }
  .bp3-file-input.bp3-file-input-has-selection .bp3-file-upload-input{
    color:#182026; }
  .bp3-dark .bp3-file-input.bp3-file-input-has-selection .bp3-file-upload-input{
    color:#f5f8fa; }
  .bp3-file-input.bp3-fill{
    width:100%; }
  .bp3-file-input.bp3-large,
  .bp3-large .bp3-file-input{
    height:40px; }
  .bp3-file-input .bp3-file-upload-input-custom-text::after{
    content:attr(bp3-button-text); }

.bp3-file-upload-input{
  -webkit-appearance:none;
     -moz-appearance:none;
          appearance:none;
  background:#ffffff;
  border:none;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
  color:#182026;
  font-size:14px;
  font-weight:400;
  height:30px;
  line-height:30px;
  outline:none;
  padding:0 10px;
  -webkit-transition:-webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:-webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  vertical-align:middle;
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  word-wrap:normal;
  color:rgba(92, 112, 128, 0.6);
  left:0;
  padding-right:80px;
  position:absolute;
  right:0;
  top:0;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-file-upload-input::-webkit-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-file-upload-input::-moz-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-file-upload-input:-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-file-upload-input::-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-file-upload-input::placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-file-upload-input:focus, .bp3-file-upload-input.bp3-active{
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-file-upload-input[type="search"], .bp3-file-upload-input.bp3-round{
    border-radius:30px;
    -webkit-box-sizing:border-box;
            box-sizing:border-box;
    padding-left:10px; }
  .bp3-file-upload-input[readonly]{
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15); }
  .bp3-file-upload-input:disabled, .bp3-file-upload-input.bp3-disabled{
    background:rgba(206, 217, 224, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed;
    resize:none; }
  .bp3-file-upload-input::after{
    background-color:#f5f8fa;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.8)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0));
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
    color:#182026;
    min-height:24px;
    min-width:24px;
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
    word-wrap:normal;
    border-radius:3px;
    content:"Browse";
    line-height:24px;
    margin:3px;
    position:absolute;
    right:0;
    text-align:center;
    top:0;
    width:70px; }
    .bp3-file-upload-input::after:hover{
      background-clip:padding-box;
      background-color:#ebf1f5;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
    .bp3-file-upload-input::after:active, .bp3-file-upload-input::after.bp3-active{
      background-color:#d8e1e8;
      background-image:none;
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-file-upload-input::after:disabled, .bp3-file-upload-input::after.bp3-disabled{
      background-color:rgba(206, 217, 224, 0.5);
      background-image:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(92, 112, 128, 0.6);
      cursor:not-allowed;
      outline:none; }
      .bp3-file-upload-input::after:disabled.bp3-active, .bp3-file-upload-input::after:disabled.bp3-active:hover, .bp3-file-upload-input::after.bp3-disabled.bp3-active, .bp3-file-upload-input::after.bp3-disabled.bp3-active:hover{
        background:rgba(206, 217, 224, 0.7); }
  .bp3-file-upload-input:hover::after{
    background-clip:padding-box;
    background-color:#ebf1f5;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
  .bp3-file-upload-input:active::after{
    background-color:#d8e1e8;
    background-image:none;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-large .bp3-file-upload-input{
    font-size:16px;
    height:40px;
    line-height:40px;
    padding-right:95px; }
    .bp3-large .bp3-file-upload-input[type="search"], .bp3-large .bp3-file-upload-input.bp3-round{
      padding:0 15px; }
    .bp3-large .bp3-file-upload-input::after{
      min-height:30px;
      min-width:30px;
      line-height:30px;
      margin:5px;
      width:85px; }
  .bp3-dark .bp3-file-upload-input{
    background:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa;
    color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input::-webkit-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input::-moz-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input:-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input::-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input::placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input:focus{
      -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-file-upload-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-file-upload-input:disabled, .bp3-dark .bp3-file-upload-input.bp3-disabled{
      background:rgba(57, 75, 89, 0.5);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-file-upload-input::after{
      background-color:#394b59;
      background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.05)), to(rgba(255, 255, 255, 0)));
      background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
      color:#f5f8fa; }
      .bp3-dark .bp3-file-upload-input::after:hover, .bp3-dark .bp3-file-upload-input::after:active, .bp3-dark .bp3-file-upload-input::after.bp3-active{
        color:#f5f8fa; }
      .bp3-dark .bp3-file-upload-input::after:hover{
        background-color:#30404d;
        -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
                box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-file-upload-input::after:active, .bp3-dark .bp3-file-upload-input::after.bp3-active{
        background-color:#202b33;
        background-image:none;
        -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
                box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
      .bp3-dark .bp3-file-upload-input::after:disabled, .bp3-dark .bp3-file-upload-input::after.bp3-disabled{
        background-color:rgba(57, 75, 89, 0.5);
        background-image:none;
        -webkit-box-shadow:none;
                box-shadow:none;
        color:rgba(167, 182, 194, 0.6); }
        .bp3-dark .bp3-file-upload-input::after:disabled.bp3-active, .bp3-dark .bp3-file-upload-input::after.bp3-disabled.bp3-active{
          background:rgba(57, 75, 89, 0.7); }
      .bp3-dark .bp3-file-upload-input::after .bp3-button-spinner .bp3-spinner-head{
        background:rgba(16, 22, 26, 0.5);
        stroke:#8a9ba8; }
    .bp3-dark .bp3-file-upload-input:hover::after{
      background-color:#30404d;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-file-upload-input:active::after{
      background-color:#202b33;
      background-image:none;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
.bp3-file-upload-input::after{
  -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
          box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
.bp3-form-group{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  margin:0 0 15px; }
  .bp3-form-group label.bp3-label{
    margin-bottom:5px; }
  .bp3-form-group .bp3-control{
    margin-top:7px; }
  .bp3-form-group .bp3-form-helper-text{
    color:#5c7080;
    font-size:12px;
    margin-top:5px; }
  .bp3-form-group.bp3-intent-primary .bp3-form-helper-text{
    color:#106ba3; }
  .bp3-form-group.bp3-intent-success .bp3-form-helper-text{
    color:#0d8050; }
  .bp3-form-group.bp3-intent-warning .bp3-form-helper-text{
    color:#bf7326; }
  .bp3-form-group.bp3-intent-danger .bp3-form-helper-text{
    color:#c23030; }
  .bp3-form-group.bp3-inline{
    -webkit-box-align:start;
        -ms-flex-align:start;
            align-items:flex-start;
    -webkit-box-orient:horizontal;
    -webkit-box-direction:normal;
        -ms-flex-direction:row;
            flex-direction:row; }
    .bp3-form-group.bp3-inline.bp3-large label.bp3-label{
      line-height:40px;
      margin:0 10px 0 0; }
    .bp3-form-group.bp3-inline label.bp3-label{
      line-height:30px;
      margin:0 10px 0 0; }
  .bp3-form-group.bp3-disabled .bp3-label,
  .bp3-form-group.bp3-disabled .bp3-text-muted,
  .bp3-form-group.bp3-disabled .bp3-form-helper-text{
    color:rgba(92, 112, 128, 0.6) !important; }
  .bp3-dark .bp3-form-group.bp3-intent-primary .bp3-form-helper-text{
    color:#48aff0; }
  .bp3-dark .bp3-form-group.bp3-intent-success .bp3-form-helper-text{
    color:#3dcc91; }
  .bp3-dark .bp3-form-group.bp3-intent-warning .bp3-form-helper-text{
    color:#ffb366; }
  .bp3-dark .bp3-form-group.bp3-intent-danger .bp3-form-helper-text{
    color:#ff7373; }
  .bp3-dark .bp3-form-group .bp3-form-helper-text{
    color:#a7b6c2; }
  .bp3-dark .bp3-form-group.bp3-disabled .bp3-label,
  .bp3-dark .bp3-form-group.bp3-disabled .bp3-text-muted,
  .bp3-dark .bp3-form-group.bp3-disabled .bp3-form-helper-text{
    color:rgba(167, 182, 194, 0.6) !important; }
.bp3-input-group{
  display:block;
  position:relative; }
  .bp3-input-group .bp3-input{
    position:relative;
    width:100%; }
    .bp3-input-group .bp3-input:not(:first-child){
      padding-left:30px; }
    .bp3-input-group .bp3-input:not(:last-child){
      padding-right:30px; }
  .bp3-input-group .bp3-input-action,
  .bp3-input-group > .bp3-input-left-container,
  .bp3-input-group > .bp3-button,
  .bp3-input-group > .bp3-icon{
    position:absolute;
    top:0; }
    .bp3-input-group .bp3-input-action:first-child,
    .bp3-input-group > .bp3-input-left-container:first-child,
    .bp3-input-group > .bp3-button:first-child,
    .bp3-input-group > .bp3-icon:first-child{
      left:0; }
    .bp3-input-group .bp3-input-action:last-child,
    .bp3-input-group > .bp3-input-left-container:last-child,
    .bp3-input-group > .bp3-button:last-child,
    .bp3-input-group > .bp3-icon:last-child{
      right:0; }
  .bp3-input-group .bp3-button{
    min-height:24px;
    min-width:24px;
    margin:3px;
    padding:0 7px; }
    .bp3-input-group .bp3-button:empty{
      padding:0; }
  .bp3-input-group > .bp3-input-left-container,
  .bp3-input-group > .bp3-icon{
    z-index:1; }
  .bp3-input-group > .bp3-input-left-container > .bp3-icon,
  .bp3-input-group > .bp3-icon{
    color:#5c7080; }
    .bp3-input-group > .bp3-input-left-container > .bp3-icon:empty,
    .bp3-input-group > .bp3-icon:empty{
      font-family:"Icons16", sans-serif;
      font-size:16px;
      font-style:normal;
      font-weight:400;
      line-height:1;
      -moz-osx-font-smoothing:grayscale;
      -webkit-font-smoothing:antialiased; }
  .bp3-input-group > .bp3-input-left-container > .bp3-icon,
  .bp3-input-group > .bp3-icon,
  .bp3-input-group .bp3-input-action > .bp3-spinner{
    margin:7px; }
  .bp3-input-group .bp3-tag{
    margin:5px; }
  .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:not(:hover):not(:focus),
  .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:not(:hover):not(:focus){
    color:#5c7080; }
    .bp3-dark .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:not(:hover):not(:focus), .bp3-dark
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:not(:hover):not(:focus){
      color:#a7b6c2; }
    .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon, .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon-standard, .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon-large,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon-standard,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:not(:hover):not(:focus) .bp3-icon-large{
      color:#5c7080; }
  .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:disabled,
  .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:disabled{
    color:rgba(92, 112, 128, 0.6) !important; }
    .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:disabled .bp3-icon, .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:disabled .bp3-icon-standard, .bp3-input-group .bp3-input:not(:focus) + .bp3-button.bp3-minimal:disabled .bp3-icon-large,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:disabled .bp3-icon,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:disabled .bp3-icon-standard,
    .bp3-input-group .bp3-input:not(:focus) + .bp3-input-action .bp3-button.bp3-minimal:disabled .bp3-icon-large{
      color:rgba(92, 112, 128, 0.6) !important; }
  .bp3-input-group.bp3-disabled{
    cursor:not-allowed; }
    .bp3-input-group.bp3-disabled .bp3-icon{
      color:rgba(92, 112, 128, 0.6); }
  .bp3-input-group.bp3-large .bp3-button{
    min-height:30px;
    min-width:30px;
    margin:5px; }
  .bp3-input-group.bp3-large > .bp3-input-left-container > .bp3-icon,
  .bp3-input-group.bp3-large > .bp3-icon,
  .bp3-input-group.bp3-large .bp3-input-action > .bp3-spinner{
    margin:12px; }
  .bp3-input-group.bp3-large .bp3-input{
    font-size:16px;
    height:40px;
    line-height:40px; }
    .bp3-input-group.bp3-large .bp3-input[type="search"], .bp3-input-group.bp3-large .bp3-input.bp3-round{
      padding:0 15px; }
    .bp3-input-group.bp3-large .bp3-input:not(:first-child){
      padding-left:40px; }
    .bp3-input-group.bp3-large .bp3-input:not(:last-child){
      padding-right:40px; }
  .bp3-input-group.bp3-small .bp3-button{
    min-height:20px;
    min-width:20px;
    margin:2px; }
  .bp3-input-group.bp3-small .bp3-tag{
    min-height:20px;
    min-width:20px;
    margin:2px; }
  .bp3-input-group.bp3-small > .bp3-input-left-container > .bp3-icon,
  .bp3-input-group.bp3-small > .bp3-icon,
  .bp3-input-group.bp3-small .bp3-input-action > .bp3-spinner{
    margin:4px; }
  .bp3-input-group.bp3-small .bp3-input{
    font-size:12px;
    height:24px;
    line-height:24px;
    padding-left:8px;
    padding-right:8px; }
    .bp3-input-group.bp3-small .bp3-input[type="search"], .bp3-input-group.bp3-small .bp3-input.bp3-round{
      padding:0 12px; }
    .bp3-input-group.bp3-small .bp3-input:not(:first-child){
      padding-left:24px; }
    .bp3-input-group.bp3-small .bp3-input:not(:last-child){
      padding-right:24px; }
  .bp3-input-group.bp3-fill{
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto;
    width:100%; }
  .bp3-input-group.bp3-round .bp3-button,
  .bp3-input-group.bp3-round .bp3-input,
  .bp3-input-group.bp3-round .bp3-tag{
    border-radius:30px; }
  .bp3-dark .bp3-input-group .bp3-icon{
    color:#a7b6c2; }
  .bp3-dark .bp3-input-group.bp3-disabled .bp3-icon{
    color:rgba(167, 182, 194, 0.6); }
  .bp3-input-group.bp3-intent-primary .bp3-input{
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-primary .bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-primary .bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #137cbd;
              box-shadow:inset 0 0 0 1px #137cbd; }
    .bp3-input-group.bp3-intent-primary .bp3-input:disabled, .bp3-input-group.bp3-intent-primary .bp3-input.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-input-group.bp3-intent-primary > .bp3-icon{
    color:#106ba3; }
    .bp3-dark .bp3-input-group.bp3-intent-primary > .bp3-icon{
      color:#48aff0; }
  .bp3-input-group.bp3-intent-success .bp3-input{
    -webkit-box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-success .bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-success .bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #0f9960;
              box-shadow:inset 0 0 0 1px #0f9960; }
    .bp3-input-group.bp3-intent-success .bp3-input:disabled, .bp3-input-group.bp3-intent-success .bp3-input.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-input-group.bp3-intent-success > .bp3-icon{
    color:#0d8050; }
    .bp3-dark .bp3-input-group.bp3-intent-success > .bp3-icon{
      color:#3dcc91; }
  .bp3-input-group.bp3-intent-warning .bp3-input{
    -webkit-box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-warning .bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-warning .bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #d9822b;
              box-shadow:inset 0 0 0 1px #d9822b; }
    .bp3-input-group.bp3-intent-warning .bp3-input:disabled, .bp3-input-group.bp3-intent-warning .bp3-input.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-input-group.bp3-intent-warning > .bp3-icon{
    color:#bf7326; }
    .bp3-dark .bp3-input-group.bp3-intent-warning > .bp3-icon{
      color:#ffb366; }
  .bp3-input-group.bp3-intent-danger .bp3-input{
    -webkit-box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-danger .bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input-group.bp3-intent-danger .bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #db3737;
              box-shadow:inset 0 0 0 1px #db3737; }
    .bp3-input-group.bp3-intent-danger .bp3-input:disabled, .bp3-input-group.bp3-intent-danger .bp3-input.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-input-group.bp3-intent-danger > .bp3-icon{
    color:#c23030; }
    .bp3-dark .bp3-input-group.bp3-intent-danger > .bp3-icon{
      color:#ff7373; }
.bp3-input{
  -webkit-appearance:none;
     -moz-appearance:none;
          appearance:none;
  background:#ffffff;
  border:none;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
  color:#182026;
  font-size:14px;
  font-weight:400;
  height:30px;
  line-height:30px;
  outline:none;
  padding:0 10px;
  -webkit-transition:-webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:-webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-box-shadow 100ms cubic-bezier(0.4, 1, 0.75, 0.9);
  vertical-align:middle; }
  .bp3-input::-webkit-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input::-moz-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input:-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input::-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input::placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input:focus, .bp3-input.bp3-active{
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-input[type="search"], .bp3-input.bp3-round{
    border-radius:30px;
    -webkit-box-sizing:border-box;
            box-sizing:border-box;
    padding-left:10px; }
  .bp3-input[readonly]{
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.15); }
  .bp3-input:disabled, .bp3-input.bp3-disabled{
    background:rgba(206, 217, 224, 0.5);
    -webkit-box-shadow:none;
            box-shadow:none;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed;
    resize:none; }
  .bp3-input.bp3-large{
    font-size:16px;
    height:40px;
    line-height:40px; }
    .bp3-input.bp3-large[type="search"], .bp3-input.bp3-large.bp3-round{
      padding:0 15px; }
  .bp3-input.bp3-small{
    font-size:12px;
    height:24px;
    line-height:24px;
    padding-left:8px;
    padding-right:8px; }
    .bp3-input.bp3-small[type="search"], .bp3-input.bp3-small.bp3-round{
      padding:0 12px; }
  .bp3-input.bp3-fill{
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto;
    width:100%; }
  .bp3-dark .bp3-input{
    background:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }
    .bp3-dark .bp3-input::-webkit-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-input::-moz-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-input:-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-input::-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-input::placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-input:disabled, .bp3-dark .bp3-input.bp3-disabled{
      background:rgba(57, 75, 89, 0.5);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(167, 182, 194, 0.6); }
  .bp3-input.bp3-intent-primary{
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-primary:focus{
      -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-primary[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #137cbd;
              box-shadow:inset 0 0 0 1px #137cbd; }
    .bp3-input.bp3-intent-primary:disabled, .bp3-input.bp3-intent-primary.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
    .bp3-dark .bp3-input.bp3-intent-primary{
      -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px #137cbd, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-primary:focus{
        -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
                box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-primary[readonly]{
        -webkit-box-shadow:inset 0 0 0 1px #137cbd;
                box-shadow:inset 0 0 0 1px #137cbd; }
      .bp3-dark .bp3-input.bp3-intent-primary:disabled, .bp3-dark .bp3-input.bp3-intent-primary.bp3-disabled{
        -webkit-box-shadow:none;
                box-shadow:none; }
  .bp3-input.bp3-intent-success{
    -webkit-box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-success:focus{
      -webkit-box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-success[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #0f9960;
              box-shadow:inset 0 0 0 1px #0f9960; }
    .bp3-input.bp3-intent-success:disabled, .bp3-input.bp3-intent-success.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
    .bp3-dark .bp3-input.bp3-intent-success{
      -webkit-box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), 0 0 0 0 rgba(15, 153, 96, 0), inset 0 0 0 1px #0f9960, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-success:focus{
        -webkit-box-shadow:0 0 0 1px #0f9960, 0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
                box-shadow:0 0 0 1px #0f9960, 0 0 0 1px #0f9960, 0 0 0 3px rgba(15, 153, 96, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-success[readonly]{
        -webkit-box-shadow:inset 0 0 0 1px #0f9960;
                box-shadow:inset 0 0 0 1px #0f9960; }
      .bp3-dark .bp3-input.bp3-intent-success:disabled, .bp3-dark .bp3-input.bp3-intent-success.bp3-disabled{
        -webkit-box-shadow:none;
                box-shadow:none; }
  .bp3-input.bp3-intent-warning{
    -webkit-box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-warning:focus{
      -webkit-box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-warning[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #d9822b;
              box-shadow:inset 0 0 0 1px #d9822b; }
    .bp3-input.bp3-intent-warning:disabled, .bp3-input.bp3-intent-warning.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
    .bp3-dark .bp3-input.bp3-intent-warning{
      -webkit-box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), 0 0 0 0 rgba(217, 130, 43, 0), inset 0 0 0 1px #d9822b, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-warning:focus{
        -webkit-box-shadow:0 0 0 1px #d9822b, 0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
                box-shadow:0 0 0 1px #d9822b, 0 0 0 1px #d9822b, 0 0 0 3px rgba(217, 130, 43, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-warning[readonly]{
        -webkit-box-shadow:inset 0 0 0 1px #d9822b;
                box-shadow:inset 0 0 0 1px #d9822b; }
      .bp3-dark .bp3-input.bp3-intent-warning:disabled, .bp3-dark .bp3-input.bp3-intent-warning.bp3-disabled{
        -webkit-box-shadow:none;
                box-shadow:none; }
  .bp3-input.bp3-intent-danger{
    -webkit-box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.15), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-danger:focus{
      -webkit-box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-input.bp3-intent-danger[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px #db3737;
              box-shadow:inset 0 0 0 1px #db3737; }
    .bp3-input.bp3-intent-danger:disabled, .bp3-input.bp3-intent-danger.bp3-disabled{
      -webkit-box-shadow:none;
              box-shadow:none; }
    .bp3-dark .bp3-input.bp3-intent-danger{
      -webkit-box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), 0 0 0 0 rgba(219, 55, 55, 0), inset 0 0 0 1px #db3737, inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-danger:focus{
        -webkit-box-shadow:0 0 0 1px #db3737, 0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
                box-shadow:0 0 0 1px #db3737, 0 0 0 1px #db3737, 0 0 0 3px rgba(219, 55, 55, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
      .bp3-dark .bp3-input.bp3-intent-danger[readonly]{
        -webkit-box-shadow:inset 0 0 0 1px #db3737;
                box-shadow:inset 0 0 0 1px #db3737; }
      .bp3-dark .bp3-input.bp3-intent-danger:disabled, .bp3-dark .bp3-input.bp3-intent-danger.bp3-disabled{
        -webkit-box-shadow:none;
                box-shadow:none; }
  .bp3-input::-ms-clear{
    display:none; }
textarea.bp3-input{
  max-width:100%;
  padding:10px; }
  textarea.bp3-input, textarea.bp3-input.bp3-large, textarea.bp3-input.bp3-small{
    height:auto;
    line-height:inherit; }
  textarea.bp3-input.bp3-small{
    padding:8px; }
  .bp3-dark textarea.bp3-input{
    background:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), 0 0 0 0 rgba(19, 124, 189, 0), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }
    .bp3-dark textarea.bp3-input::-webkit-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark textarea.bp3-input::-moz-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark textarea.bp3-input:-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark textarea.bp3-input::-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark textarea.bp3-input::placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark textarea.bp3-input:focus{
      -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark textarea.bp3-input[readonly]{
      -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark textarea.bp3-input:disabled, .bp3-dark textarea.bp3-input.bp3-disabled{
      background:rgba(57, 75, 89, 0.5);
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(167, 182, 194, 0.6); }
label.bp3-label{
  display:block;
  margin-bottom:15px;
  margin-top:0; }
  label.bp3-label .bp3-html-select,
  label.bp3-label .bp3-input,
  label.bp3-label .bp3-select,
  label.bp3-label .bp3-slider,
  label.bp3-label .bp3-popover-wrapper{
    display:block;
    margin-top:5px;
    text-transform:none; }
  label.bp3-label .bp3-button-group{
    margin-top:5px; }
  label.bp3-label .bp3-select select,
  label.bp3-label .bp3-html-select select{
    font-weight:400;
    vertical-align:top;
    width:100%; }
  label.bp3-label.bp3-disabled,
  label.bp3-label.bp3-disabled .bp3-text-muted{
    color:rgba(92, 112, 128, 0.6); }
  label.bp3-label.bp3-inline{
    line-height:30px; }
    label.bp3-label.bp3-inline .bp3-html-select,
    label.bp3-label.bp3-inline .bp3-input,
    label.bp3-label.bp3-inline .bp3-input-group,
    label.bp3-label.bp3-inline .bp3-select,
    label.bp3-label.bp3-inline .bp3-popover-wrapper{
      display:inline-block;
      margin:0 0 0 5px;
      vertical-align:top; }
    label.bp3-label.bp3-inline .bp3-button-group{
      margin:0 0 0 5px; }
    label.bp3-label.bp3-inline .bp3-input-group .bp3-input{
      margin-left:0; }
    label.bp3-label.bp3-inline.bp3-large{
      line-height:40px; }
  label.bp3-label:not(.bp3-inline) .bp3-popover-target{
    display:block; }
  .bp3-dark label.bp3-label{
    color:#f5f8fa; }
    .bp3-dark label.bp3-label.bp3-disabled,
    .bp3-dark label.bp3-label.bp3-disabled .bp3-text-muted{
      color:rgba(167, 182, 194, 0.6); }
.bp3-numeric-input .bp3-button-group.bp3-vertical > .bp3-button{
  -webkit-box-flex:1;
      -ms-flex:1 1 14px;
          flex:1 1 14px;
  min-height:0;
  padding:0;
  width:30px; }
  .bp3-numeric-input .bp3-button-group.bp3-vertical > .bp3-button:first-child{
    border-radius:0 3px 0 0; }
  .bp3-numeric-input .bp3-button-group.bp3-vertical > .bp3-button:last-child{
    border-radius:0 0 3px 0; }

.bp3-numeric-input .bp3-button-group.bp3-vertical:first-child > .bp3-button:first-child{
  border-radius:3px 0 0 0; }

.bp3-numeric-input .bp3-button-group.bp3-vertical:first-child > .bp3-button:last-child{
  border-radius:0 0 0 3px; }

.bp3-numeric-input.bp3-large .bp3-button-group.bp3-vertical > .bp3-button{
  width:40px; }

form{
  display:block; }
.bp3-html-select select,
.bp3-select select{
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  border:none;
  border-radius:3px;
  cursor:pointer;
  font-size:14px;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  padding:5px 10px;
  text-align:left;
  vertical-align:middle;
  background-color:#f5f8fa;
  background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.8)), to(rgba(255, 255, 255, 0)));
  background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0));
  -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
          box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
  color:#182026;
  -moz-appearance:none;
  -webkit-appearance:none;
  border-radius:3px;
  height:30px;
  padding:0 25px 0 10px;
  width:100%; }
  .bp3-html-select select > *, .bp3-select select > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-html-select select > .bp3-fill, .bp3-select select > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-html-select select::before,
  .bp3-select select::before, .bp3-html-select select > *, .bp3-select select > *{
    margin-right:7px; }
  .bp3-html-select select:empty::before,
  .bp3-select select:empty::before,
  .bp3-html-select select > :last-child,
  .bp3-select select > :last-child{
    margin-right:0; }
  .bp3-html-select select:hover,
  .bp3-select select:hover{
    background-clip:padding-box;
    background-color:#ebf1f5;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
  .bp3-html-select select:active,
  .bp3-select select:active, .bp3-html-select select.bp3-active,
  .bp3-select select.bp3-active{
    background-color:#d8e1e8;
    background-image:none;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-html-select select:disabled,
  .bp3-select select:disabled, .bp3-html-select select.bp3-disabled,
  .bp3-select select.bp3-disabled{
    background-color:rgba(206, 217, 224, 0.5);
    background-image:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed;
    outline:none; }
    .bp3-html-select select:disabled.bp3-active,
    .bp3-select select:disabled.bp3-active, .bp3-html-select select:disabled.bp3-active:hover,
    .bp3-select select:disabled.bp3-active:hover, .bp3-html-select select.bp3-disabled.bp3-active,
    .bp3-select select.bp3-disabled.bp3-active, .bp3-html-select select.bp3-disabled.bp3-active:hover,
    .bp3-select select.bp3-disabled.bp3-active:hover{
      background:rgba(206, 217, 224, 0.7); }

.bp3-html-select.bp3-minimal select,
.bp3-select.bp3-minimal select{
  background:none;
  -webkit-box-shadow:none;
          box-shadow:none; }
  .bp3-html-select.bp3-minimal select:hover,
  .bp3-select.bp3-minimal select:hover{
    background:rgba(167, 182, 194, 0.3);
    -webkit-box-shadow:none;
            box-shadow:none;
    color:#182026;
    text-decoration:none; }
  .bp3-html-select.bp3-minimal select:active,
  .bp3-select.bp3-minimal select:active, .bp3-html-select.bp3-minimal select.bp3-active,
  .bp3-select.bp3-minimal select.bp3-active{
    background:rgba(115, 134, 148, 0.3);
    -webkit-box-shadow:none;
            box-shadow:none;
    color:#182026; }
  .bp3-html-select.bp3-minimal select:disabled,
  .bp3-select.bp3-minimal select:disabled, .bp3-html-select.bp3-minimal select:disabled:hover,
  .bp3-select.bp3-minimal select:disabled:hover, .bp3-html-select.bp3-minimal select.bp3-disabled,
  .bp3-select.bp3-minimal select.bp3-disabled, .bp3-html-select.bp3-minimal select.bp3-disabled:hover,
  .bp3-select.bp3-minimal select.bp3-disabled:hover{
    background:none;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed; }
    .bp3-html-select.bp3-minimal select:disabled.bp3-active,
    .bp3-select.bp3-minimal select:disabled.bp3-active, .bp3-html-select.bp3-minimal select:disabled:hover.bp3-active,
    .bp3-select.bp3-minimal select:disabled:hover.bp3-active, .bp3-html-select.bp3-minimal select.bp3-disabled.bp3-active,
    .bp3-select.bp3-minimal select.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal select.bp3-disabled:hover.bp3-active,
    .bp3-select.bp3-minimal select.bp3-disabled:hover.bp3-active{
      background:rgba(115, 134, 148, 0.3); }
  .bp3-dark .bp3-html-select.bp3-minimal select, .bp3-html-select.bp3-minimal .bp3-dark select,
  .bp3-dark .bp3-select.bp3-minimal select, .bp3-select.bp3-minimal .bp3-dark select{
    background:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    color:inherit; }
    .bp3-dark .bp3-html-select.bp3-minimal select:hover, .bp3-html-select.bp3-minimal .bp3-dark select:hover,
    .bp3-dark .bp3-select.bp3-minimal select:hover, .bp3-select.bp3-minimal .bp3-dark select:hover, .bp3-dark .bp3-html-select.bp3-minimal select:active, .bp3-html-select.bp3-minimal .bp3-dark select:active,
    .bp3-dark .bp3-select.bp3-minimal select:active, .bp3-select.bp3-minimal .bp3-dark select:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-active,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-active{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none; }
    .bp3-dark .bp3-html-select.bp3-minimal select:hover, .bp3-html-select.bp3-minimal .bp3-dark select:hover,
    .bp3-dark .bp3-select.bp3-minimal select:hover, .bp3-select.bp3-minimal .bp3-dark select:hover{
      background:rgba(138, 155, 168, 0.15); }
    .bp3-dark .bp3-html-select.bp3-minimal select:active, .bp3-html-select.bp3-minimal .bp3-dark select:active,
    .bp3-dark .bp3-select.bp3-minimal select:active, .bp3-select.bp3-minimal .bp3-dark select:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-active,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-active{
      background:rgba(138, 155, 168, 0.3);
      color:#f5f8fa; }
    .bp3-dark .bp3-html-select.bp3-minimal select:disabled, .bp3-html-select.bp3-minimal .bp3-dark select:disabled,
    .bp3-dark .bp3-select.bp3-minimal select:disabled, .bp3-select.bp3-minimal .bp3-dark select:disabled, .bp3-dark .bp3-html-select.bp3-minimal select:disabled:hover, .bp3-html-select.bp3-minimal .bp3-dark select:disabled:hover,
    .bp3-dark .bp3-select.bp3-minimal select:disabled:hover, .bp3-select.bp3-minimal .bp3-dark select:disabled:hover, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-disabled,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-disabled, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-disabled:hover, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-disabled:hover,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-disabled:hover, .bp3-select.bp3-minimal .bp3-dark select.bp3-disabled:hover{
      background:none;
      color:rgba(167, 182, 194, 0.6);
      cursor:not-allowed; }
      .bp3-dark .bp3-html-select.bp3-minimal select:disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select:disabled.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select:disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select:disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select:disabled:hover.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select:disabled:hover.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select:disabled:hover.bp3-active, .bp3-select.bp3-minimal .bp3-dark select:disabled:hover.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-disabled.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-disabled:hover.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-disabled:hover.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-disabled:hover.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-disabled:hover.bp3-active{
        background:rgba(138, 155, 168, 0.3); }
  .bp3-html-select.bp3-minimal select.bp3-intent-primary,
  .bp3-select.bp3-minimal select.bp3-intent-primary{
    color:#106ba3; }
    .bp3-html-select.bp3-minimal select.bp3-intent-primary:hover,
    .bp3-select.bp3-minimal select.bp3-intent-primary:hover, .bp3-html-select.bp3-minimal select.bp3-intent-primary:active,
    .bp3-select.bp3-minimal select.bp3-intent-primary:active, .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-active{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#106ba3; }
    .bp3-html-select.bp3-minimal select.bp3-intent-primary:hover,
    .bp3-select.bp3-minimal select.bp3-intent-primary:hover{
      background:rgba(19, 124, 189, 0.15);
      color:#106ba3; }
    .bp3-html-select.bp3-minimal select.bp3-intent-primary:active,
    .bp3-select.bp3-minimal select.bp3-intent-primary:active, .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-active{
      background:rgba(19, 124, 189, 0.3);
      color:#106ba3; }
    .bp3-html-select.bp3-minimal select.bp3-intent-primary:disabled,
    .bp3-select.bp3-minimal select.bp3-intent-primary:disabled, .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-disabled,
    .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-disabled{
      background:none;
      color:rgba(16, 107, 163, 0.5); }
      .bp3-html-select.bp3-minimal select.bp3-intent-primary:disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-primary:disabled.bp3-active, .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-disabled.bp3-active{
        background:rgba(19, 124, 189, 0.3); }
    .bp3-html-select.bp3-minimal select.bp3-intent-primary .bp3-button-spinner .bp3-spinner-head, .bp3-select.bp3-minimal select.bp3-intent-primary .bp3-button-spinner .bp3-spinner-head{
      stroke:#106ba3; }
    .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary{
      color:#48aff0; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary:hover, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary:hover,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary:hover, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary:hover{
        background:rgba(19, 124, 189, 0.2);
        color:#48aff0; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary:active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary:active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary:active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-active{
        background:rgba(19, 124, 189, 0.3);
        color:#48aff0; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary:disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary:disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary:disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary:disabled, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-disabled{
        background:none;
        color:rgba(72, 175, 240, 0.5); }
        .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary:disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary:disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary:disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary:disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-primary.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-primary.bp3-disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-primary.bp3-disabled.bp3-active{
          background:rgba(19, 124, 189, 0.3); }
  .bp3-html-select.bp3-minimal select.bp3-intent-success,
  .bp3-select.bp3-minimal select.bp3-intent-success{
    color:#0d8050; }
    .bp3-html-select.bp3-minimal select.bp3-intent-success:hover,
    .bp3-select.bp3-minimal select.bp3-intent-success:hover, .bp3-html-select.bp3-minimal select.bp3-intent-success:active,
    .bp3-select.bp3-minimal select.bp3-intent-success:active, .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-success.bp3-active{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#0d8050; }
    .bp3-html-select.bp3-minimal select.bp3-intent-success:hover,
    .bp3-select.bp3-minimal select.bp3-intent-success:hover{
      background:rgba(15, 153, 96, 0.15);
      color:#0d8050; }
    .bp3-html-select.bp3-minimal select.bp3-intent-success:active,
    .bp3-select.bp3-minimal select.bp3-intent-success:active, .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-success.bp3-active{
      background:rgba(15, 153, 96, 0.3);
      color:#0d8050; }
    .bp3-html-select.bp3-minimal select.bp3-intent-success:disabled,
    .bp3-select.bp3-minimal select.bp3-intent-success:disabled, .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-disabled,
    .bp3-select.bp3-minimal select.bp3-intent-success.bp3-disabled{
      background:none;
      color:rgba(13, 128, 80, 0.5); }
      .bp3-html-select.bp3-minimal select.bp3-intent-success:disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-success:disabled.bp3-active, .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-success.bp3-disabled.bp3-active{
        background:rgba(15, 153, 96, 0.3); }
    .bp3-html-select.bp3-minimal select.bp3-intent-success .bp3-button-spinner .bp3-spinner-head, .bp3-select.bp3-minimal select.bp3-intent-success .bp3-button-spinner .bp3-spinner-head{
      stroke:#0d8050; }
    .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success{
      color:#3dcc91; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success:hover, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success:hover,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success:hover, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success:hover{
        background:rgba(15, 153, 96, 0.2);
        color:#3dcc91; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success:active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success:active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success:active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-active{
        background:rgba(15, 153, 96, 0.3);
        color:#3dcc91; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success:disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success:disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success:disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success:disabled, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success.bp3-disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-disabled{
        background:none;
        color:rgba(61, 204, 145, 0.5); }
        .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success:disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success:disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success:disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success:disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-success.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-success.bp3-disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-success.bp3-disabled.bp3-active{
          background:rgba(15, 153, 96, 0.3); }
  .bp3-html-select.bp3-minimal select.bp3-intent-warning,
  .bp3-select.bp3-minimal select.bp3-intent-warning{
    color:#bf7326; }
    .bp3-html-select.bp3-minimal select.bp3-intent-warning:hover,
    .bp3-select.bp3-minimal select.bp3-intent-warning:hover, .bp3-html-select.bp3-minimal select.bp3-intent-warning:active,
    .bp3-select.bp3-minimal select.bp3-intent-warning:active, .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-active{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#bf7326; }
    .bp3-html-select.bp3-minimal select.bp3-intent-warning:hover,
    .bp3-select.bp3-minimal select.bp3-intent-warning:hover{
      background:rgba(217, 130, 43, 0.15);
      color:#bf7326; }
    .bp3-html-select.bp3-minimal select.bp3-intent-warning:active,
    .bp3-select.bp3-minimal select.bp3-intent-warning:active, .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-active{
      background:rgba(217, 130, 43, 0.3);
      color:#bf7326; }
    .bp3-html-select.bp3-minimal select.bp3-intent-warning:disabled,
    .bp3-select.bp3-minimal select.bp3-intent-warning:disabled, .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-disabled,
    .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-disabled{
      background:none;
      color:rgba(191, 115, 38, 0.5); }
      .bp3-html-select.bp3-minimal select.bp3-intent-warning:disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-warning:disabled.bp3-active, .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-disabled.bp3-active{
        background:rgba(217, 130, 43, 0.3); }
    .bp3-html-select.bp3-minimal select.bp3-intent-warning .bp3-button-spinner .bp3-spinner-head, .bp3-select.bp3-minimal select.bp3-intent-warning .bp3-button-spinner .bp3-spinner-head{
      stroke:#bf7326; }
    .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning{
      color:#ffb366; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning:hover, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning:hover,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning:hover, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning:hover{
        background:rgba(217, 130, 43, 0.2);
        color:#ffb366; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning:active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning:active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning:active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-active{
        background:rgba(217, 130, 43, 0.3);
        color:#ffb366; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning:disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning:disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning:disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning:disabled, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-disabled{
        background:none;
        color:rgba(255, 179, 102, 0.5); }
        .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning:disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning:disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning:disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning:disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-warning.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-warning.bp3-disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-warning.bp3-disabled.bp3-active{
          background:rgba(217, 130, 43, 0.3); }
  .bp3-html-select.bp3-minimal select.bp3-intent-danger,
  .bp3-select.bp3-minimal select.bp3-intent-danger{
    color:#c23030; }
    .bp3-html-select.bp3-minimal select.bp3-intent-danger:hover,
    .bp3-select.bp3-minimal select.bp3-intent-danger:hover, .bp3-html-select.bp3-minimal select.bp3-intent-danger:active,
    .bp3-select.bp3-minimal select.bp3-intent-danger:active, .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-active{
      background:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:#c23030; }
    .bp3-html-select.bp3-minimal select.bp3-intent-danger:hover,
    .bp3-select.bp3-minimal select.bp3-intent-danger:hover{
      background:rgba(219, 55, 55, 0.15);
      color:#c23030; }
    .bp3-html-select.bp3-minimal select.bp3-intent-danger:active,
    .bp3-select.bp3-minimal select.bp3-intent-danger:active, .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-active,
    .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-active{
      background:rgba(219, 55, 55, 0.3);
      color:#c23030; }
    .bp3-html-select.bp3-minimal select.bp3-intent-danger:disabled,
    .bp3-select.bp3-minimal select.bp3-intent-danger:disabled, .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-disabled,
    .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-disabled{
      background:none;
      color:rgba(194, 48, 48, 0.5); }
      .bp3-html-select.bp3-minimal select.bp3-intent-danger:disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-danger:disabled.bp3-active, .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-disabled.bp3-active,
      .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-disabled.bp3-active{
        background:rgba(219, 55, 55, 0.3); }
    .bp3-html-select.bp3-minimal select.bp3-intent-danger .bp3-button-spinner .bp3-spinner-head, .bp3-select.bp3-minimal select.bp3-intent-danger .bp3-button-spinner .bp3-spinner-head{
      stroke:#c23030; }
    .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger,
    .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger{
      color:#ff7373; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger:hover, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger:hover,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger:hover, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger:hover{
        background:rgba(219, 55, 55, 0.2);
        color:#ff7373; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger:active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger:active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger:active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger:active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-active,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-active{
        background:rgba(219, 55, 55, 0.3);
        color:#ff7373; }
      .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger:disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger:disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger:disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger:disabled, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-disabled, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-disabled,
      .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-disabled, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-disabled{
        background:none;
        color:rgba(255, 115, 115, 0.5); }
        .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger:disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger:disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger:disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger:disabled.bp3-active, .bp3-dark .bp3-html-select.bp3-minimal select.bp3-intent-danger.bp3-disabled.bp3-active, .bp3-html-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-disabled.bp3-active,
        .bp3-dark .bp3-select.bp3-minimal select.bp3-intent-danger.bp3-disabled.bp3-active, .bp3-select.bp3-minimal .bp3-dark select.bp3-intent-danger.bp3-disabled.bp3-active{
          background:rgba(219, 55, 55, 0.3); }

.bp3-html-select.bp3-large select,
.bp3-select.bp3-large select{
  font-size:16px;
  height:40px;
  padding-right:35px; }

.bp3-dark .bp3-html-select select, .bp3-dark .bp3-select select{
  background-color:#394b59;
  background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.05)), to(rgba(255, 255, 255, 0)));
  background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
  color:#f5f8fa; }
  .bp3-dark .bp3-html-select select:hover, .bp3-dark .bp3-select select:hover, .bp3-dark .bp3-html-select select:active, .bp3-dark .bp3-select select:active, .bp3-dark .bp3-html-select select.bp3-active, .bp3-dark .bp3-select select.bp3-active{
    color:#f5f8fa; }
  .bp3-dark .bp3-html-select select:hover, .bp3-dark .bp3-select select:hover{
    background-color:#30404d;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-html-select select:active, .bp3-dark .bp3-select select:active, .bp3-dark .bp3-html-select select.bp3-active, .bp3-dark .bp3-select select.bp3-active{
    background-color:#202b33;
    background-image:none;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-html-select select:disabled, .bp3-dark .bp3-select select:disabled, .bp3-dark .bp3-html-select select.bp3-disabled, .bp3-dark .bp3-select select.bp3-disabled{
    background-color:rgba(57, 75, 89, 0.5);
    background-image:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-html-select select:disabled.bp3-active, .bp3-dark .bp3-select select:disabled.bp3-active, .bp3-dark .bp3-html-select select.bp3-disabled.bp3-active, .bp3-dark .bp3-select select.bp3-disabled.bp3-active{
      background:rgba(57, 75, 89, 0.7); }
  .bp3-dark .bp3-html-select select .bp3-button-spinner .bp3-spinner-head, .bp3-dark .bp3-select select .bp3-button-spinner .bp3-spinner-head{
    background:rgba(16, 22, 26, 0.5);
    stroke:#8a9ba8; }

.bp3-html-select select:disabled,
.bp3-select select:disabled{
  background-color:rgba(206, 217, 224, 0.5);
  -webkit-box-shadow:none;
          box-shadow:none;
  color:rgba(92, 112, 128, 0.6);
  cursor:not-allowed; }

.bp3-html-select .bp3-icon,
.bp3-select .bp3-icon, .bp3-select::after{
  color:#5c7080;
  pointer-events:none;
  position:absolute;
  right:7px;
  top:7px; }
  .bp3-html-select .bp3-disabled.bp3-icon,
  .bp3-select .bp3-disabled.bp3-icon, .bp3-disabled.bp3-select::after{
    color:rgba(92, 112, 128, 0.6); }
.bp3-html-select,
.bp3-select{
  display:inline-block;
  letter-spacing:normal;
  position:relative;
  vertical-align:middle; }
  .bp3-html-select select::-ms-expand,
  .bp3-select select::-ms-expand{
    display:none; }
  .bp3-html-select .bp3-icon,
  .bp3-select .bp3-icon{
    color:#5c7080; }
    .bp3-html-select .bp3-icon:hover,
    .bp3-select .bp3-icon:hover{
      color:#182026; }
    .bp3-dark .bp3-html-select .bp3-icon, .bp3-dark
    .bp3-select .bp3-icon{
      color:#a7b6c2; }
      .bp3-dark .bp3-html-select .bp3-icon:hover, .bp3-dark
      .bp3-select .bp3-icon:hover{
        color:#f5f8fa; }
  .bp3-html-select.bp3-large::after,
  .bp3-html-select.bp3-large .bp3-icon,
  .bp3-select.bp3-large::after,
  .bp3-select.bp3-large .bp3-icon{
    right:12px;
    top:12px; }
  .bp3-html-select.bp3-fill,
  .bp3-html-select.bp3-fill select,
  .bp3-select.bp3-fill,
  .bp3-select.bp3-fill select{
    width:100%; }
  .bp3-dark .bp3-html-select option, .bp3-dark
  .bp3-select option{
    background-color:#30404d;
    color:#f5f8fa; }
  .bp3-dark .bp3-html-select option:disabled, .bp3-dark
  .bp3-select option:disabled{
    color:rgba(167, 182, 194, 0.6); }
  .bp3-dark .bp3-html-select::after, .bp3-dark
  .bp3-select::after{
    color:#a7b6c2; }

.bp3-select::after{
  font-family:"Icons16", sans-serif;
  font-size:16px;
  font-style:normal;
  font-weight:400;
  line-height:1;
  -moz-osx-font-smoothing:grayscale;
  -webkit-font-smoothing:antialiased;
  content:""; }
.bp3-running-text table, table.bp3-html-table{
  border-spacing:0;
  font-size:14px; }
  .bp3-running-text table th, table.bp3-html-table th,
  .bp3-running-text table td,
  table.bp3-html-table td{
    padding:11px;
    text-align:left;
    vertical-align:top; }
  .bp3-running-text table th, table.bp3-html-table th{
    color:#182026;
    font-weight:600; }
  
  .bp3-running-text table td,
  table.bp3-html-table td{
    color:#182026; }
  .bp3-running-text table tbody tr:first-child th, table.bp3-html-table tbody tr:first-child th,
  .bp3-running-text table tbody tr:first-child td,
  table.bp3-html-table tbody tr:first-child td,
  .bp3-running-text table tfoot tr:first-child th,
  table.bp3-html-table tfoot tr:first-child th,
  .bp3-running-text table tfoot tr:first-child td,
  table.bp3-html-table tfoot tr:first-child td{
    -webkit-box-shadow:inset 0 1px 0 0 rgba(16, 22, 26, 0.15);
            box-shadow:inset 0 1px 0 0 rgba(16, 22, 26, 0.15); }
  .bp3-dark .bp3-running-text table th, .bp3-running-text .bp3-dark table th, .bp3-dark table.bp3-html-table th{
    color:#f5f8fa; }
  .bp3-dark .bp3-running-text table td, .bp3-running-text .bp3-dark table td, .bp3-dark table.bp3-html-table td{
    color:#f5f8fa; }
  .bp3-dark .bp3-running-text table tbody tr:first-child th, .bp3-running-text .bp3-dark table tbody tr:first-child th, .bp3-dark table.bp3-html-table tbody tr:first-child th,
  .bp3-dark .bp3-running-text table tbody tr:first-child td,
  .bp3-running-text .bp3-dark table tbody tr:first-child td,
  .bp3-dark table.bp3-html-table tbody tr:first-child td,
  .bp3-dark .bp3-running-text table tfoot tr:first-child th,
  .bp3-running-text .bp3-dark table tfoot tr:first-child th,
  .bp3-dark table.bp3-html-table tfoot tr:first-child th,
  .bp3-dark .bp3-running-text table tfoot tr:first-child td,
  .bp3-running-text .bp3-dark table tfoot tr:first-child td,
  .bp3-dark table.bp3-html-table tfoot tr:first-child td{
    -webkit-box-shadow:inset 0 1px 0 0 rgba(255, 255, 255, 0.15);
            box-shadow:inset 0 1px 0 0 rgba(255, 255, 255, 0.15); }

table.bp3-html-table.bp3-html-table-condensed th,
table.bp3-html-table.bp3-html-table-condensed td, table.bp3-html-table.bp3-small th,
table.bp3-html-table.bp3-small td{
  padding-bottom:6px;
  padding-top:6px; }

table.bp3-html-table.bp3-html-table-striped tbody tr:nth-child(odd) td{
  background:rgba(191, 204, 214, 0.15); }

table.bp3-html-table.bp3-html-table-bordered th:not(:first-child){
  -webkit-box-shadow:inset 1px 0 0 0 rgba(16, 22, 26, 0.15);
          box-shadow:inset 1px 0 0 0 rgba(16, 22, 26, 0.15); }

table.bp3-html-table.bp3-html-table-bordered tbody tr td,
table.bp3-html-table.bp3-html-table-bordered tfoot tr td{
  -webkit-box-shadow:inset 0 1px 0 0 rgba(16, 22, 26, 0.15);
          box-shadow:inset 0 1px 0 0 rgba(16, 22, 26, 0.15); }
  table.bp3-html-table.bp3-html-table-bordered tbody tr td:not(:first-child),
  table.bp3-html-table.bp3-html-table-bordered tfoot tr td:not(:first-child){
    -webkit-box-shadow:inset 1px 1px 0 0 rgba(16, 22, 26, 0.15);
            box-shadow:inset 1px 1px 0 0 rgba(16, 22, 26, 0.15); }

table.bp3-html-table.bp3-html-table-bordered.bp3-html-table-striped tbody tr:not(:first-child) td{
  -webkit-box-shadow:none;
          box-shadow:none; }
  table.bp3-html-table.bp3-html-table-bordered.bp3-html-table-striped tbody tr:not(:first-child) td:not(:first-child){
    -webkit-box-shadow:inset 1px 0 0 0 rgba(16, 22, 26, 0.15);
            box-shadow:inset 1px 0 0 0 rgba(16, 22, 26, 0.15); }

table.bp3-html-table.bp3-interactive tbody tr:hover td{
  background-color:rgba(191, 204, 214, 0.3);
  cursor:pointer; }

table.bp3-html-table.bp3-interactive tbody tr:active td{
  background-color:rgba(191, 204, 214, 0.4); }

.bp3-dark table.bp3-html-table{ }
  .bp3-dark table.bp3-html-table.bp3-html-table-striped tbody tr:nth-child(odd) td{
    background:rgba(92, 112, 128, 0.15); }
  .bp3-dark table.bp3-html-table.bp3-html-table-bordered th:not(:first-child){
    -webkit-box-shadow:inset 1px 0 0 0 rgba(255, 255, 255, 0.15);
            box-shadow:inset 1px 0 0 0 rgba(255, 255, 255, 0.15); }
  .bp3-dark table.bp3-html-table.bp3-html-table-bordered tbody tr td,
  .bp3-dark table.bp3-html-table.bp3-html-table-bordered tfoot tr td{
    -webkit-box-shadow:inset 0 1px 0 0 rgba(255, 255, 255, 0.15);
            box-shadow:inset 0 1px 0 0 rgba(255, 255, 255, 0.15); }
    .bp3-dark table.bp3-html-table.bp3-html-table-bordered tbody tr td:not(:first-child),
    .bp3-dark table.bp3-html-table.bp3-html-table-bordered tfoot tr td:not(:first-child){
      -webkit-box-shadow:inset 1px 1px 0 0 rgba(255, 255, 255, 0.15);
              box-shadow:inset 1px 1px 0 0 rgba(255, 255, 255, 0.15); }
  .bp3-dark table.bp3-html-table.bp3-html-table-bordered.bp3-html-table-striped tbody tr:not(:first-child) td{
    -webkit-box-shadow:inset 1px 0 0 0 rgba(255, 255, 255, 0.15);
            box-shadow:inset 1px 0 0 0 rgba(255, 255, 255, 0.15); }
    .bp3-dark table.bp3-html-table.bp3-html-table-bordered.bp3-html-table-striped tbody tr:not(:first-child) td:first-child{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-dark table.bp3-html-table.bp3-interactive tbody tr:hover td{
    background-color:rgba(92, 112, 128, 0.3);
    cursor:pointer; }
  .bp3-dark table.bp3-html-table.bp3-interactive tbody tr:active td{
    background-color:rgba(92, 112, 128, 0.4); }

.bp3-key-combo{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center; }
  .bp3-key-combo > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-key-combo > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-key-combo::before,
  .bp3-key-combo > *{
    margin-right:5px; }
  .bp3-key-combo:empty::before,
  .bp3-key-combo > :last-child{
    margin-right:0; }

.bp3-hotkey-dialog{
  padding-bottom:0;
  top:40px; }
  .bp3-hotkey-dialog .bp3-dialog-body{
    margin:0;
    padding:0; }
  .bp3-hotkey-dialog .bp3-hotkey-label{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1; }

.bp3-hotkey-column{
  margin:auto;
  max-height:80vh;
  overflow-y:auto;
  padding:30px; }
  .bp3-hotkey-column .bp3-heading{
    margin-bottom:20px; }
    .bp3-hotkey-column .bp3-heading:not(:first-child){
      margin-top:40px; }

.bp3-hotkey{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-pack:justify;
      -ms-flex-pack:justify;
          justify-content:space-between;
  margin-left:0;
  margin-right:0; }
  .bp3-hotkey:not(:last-child){
    margin-bottom:10px; }
.bp3-icon{
  display:inline-block;
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  vertical-align:text-bottom; }
  .bp3-icon:not(:empty)::before{
    content:"" !important;
    content:unset !important; }
  .bp3-icon > svg{
    display:block; }
    .bp3-icon > svg:not([fill]){
      fill:currentColor; }

.bp3-icon.bp3-intent-primary, .bp3-icon-standard.bp3-intent-primary, .bp3-icon-large.bp3-intent-primary{
  color:#106ba3; }
  .bp3-dark .bp3-icon.bp3-intent-primary, .bp3-dark .bp3-icon-standard.bp3-intent-primary, .bp3-dark .bp3-icon-large.bp3-intent-primary{
    color:#48aff0; }

.bp3-icon.bp3-intent-success, .bp3-icon-standard.bp3-intent-success, .bp3-icon-large.bp3-intent-success{
  color:#0d8050; }
  .bp3-dark .bp3-icon.bp3-intent-success, .bp3-dark .bp3-icon-standard.bp3-intent-success, .bp3-dark .bp3-icon-large.bp3-intent-success{
    color:#3dcc91; }

.bp3-icon.bp3-intent-warning, .bp3-icon-standard.bp3-intent-warning, .bp3-icon-large.bp3-intent-warning{
  color:#bf7326; }
  .bp3-dark .bp3-icon.bp3-intent-warning, .bp3-dark .bp3-icon-standard.bp3-intent-warning, .bp3-dark .bp3-icon-large.bp3-intent-warning{
    color:#ffb366; }

.bp3-icon.bp3-intent-danger, .bp3-icon-standard.bp3-intent-danger, .bp3-icon-large.bp3-intent-danger{
  color:#c23030; }
  .bp3-dark .bp3-icon.bp3-intent-danger, .bp3-dark .bp3-icon-standard.bp3-intent-danger, .bp3-dark .bp3-icon-large.bp3-intent-danger{
    color:#ff7373; }

span.bp3-icon-standard{
  font-family:"Icons16", sans-serif;
  font-size:16px;
  font-style:normal;
  font-weight:400;
  line-height:1;
  -moz-osx-font-smoothing:grayscale;
  -webkit-font-smoothing:antialiased;
  display:inline-block; }

span.bp3-icon-large{
  font-family:"Icons20", sans-serif;
  font-size:20px;
  font-style:normal;
  font-weight:400;
  line-height:1;
  -moz-osx-font-smoothing:grayscale;
  -webkit-font-smoothing:antialiased;
  display:inline-block; }

span.bp3-icon:empty{
  font-family:"Icons20";
  font-size:inherit;
  font-style:normal;
  font-weight:400;
  line-height:1; }
  span.bp3-icon:empty::before{
    -moz-osx-font-smoothing:grayscale;
    -webkit-font-smoothing:antialiased; }

.bp3-icon-add::before{
  content:""; }

.bp3-icon-add-column-left::before{
  content:""; }

.bp3-icon-add-column-right::before{
  content:""; }

.bp3-icon-add-row-bottom::before{
  content:""; }

.bp3-icon-add-row-top::before{
  content:""; }

.bp3-icon-add-to-artifact::before{
  content:""; }

.bp3-icon-add-to-folder::before{
  content:""; }

.bp3-icon-airplane::before{
  content:""; }

.bp3-icon-align-center::before{
  content:""; }

.bp3-icon-align-justify::before{
  content:""; }

.bp3-icon-align-left::before{
  content:""; }

.bp3-icon-align-right::before{
  content:""; }

.bp3-icon-alignment-bottom::before{
  content:""; }

.bp3-icon-alignment-horizontal-center::before{
  content:""; }

.bp3-icon-alignment-left::before{
  content:""; }

.bp3-icon-alignment-right::before{
  content:""; }

.bp3-icon-alignment-top::before{
  content:""; }

.bp3-icon-alignment-vertical-center::before{
  content:""; }

.bp3-icon-annotation::before{
  content:""; }

.bp3-icon-application::before{
  content:""; }

.bp3-icon-applications::before{
  content:""; }

.bp3-icon-archive::before{
  content:""; }

.bp3-icon-arrow-bottom-left::before{
  content:"↙"; }

.bp3-icon-arrow-bottom-right::before{
  content:"↘"; }

.bp3-icon-arrow-down::before{
  content:"↓"; }

.bp3-icon-arrow-left::before{
  content:"←"; }

.bp3-icon-arrow-right::before{
  content:"→"; }

.bp3-icon-arrow-top-left::before{
  content:"↖"; }

.bp3-icon-arrow-top-right::before{
  content:"↗"; }

.bp3-icon-arrow-up::before{
  content:"↑"; }

.bp3-icon-arrows-horizontal::before{
  content:"↔"; }

.bp3-icon-arrows-vertical::before{
  content:"↕"; }

.bp3-icon-asterisk::before{
  content:"*"; }

.bp3-icon-automatic-updates::before{
  content:""; }

.bp3-icon-badge::before{
  content:""; }

.bp3-icon-ban-circle::before{
  content:""; }

.bp3-icon-bank-account::before{
  content:""; }

.bp3-icon-barcode::before{
  content:""; }

.bp3-icon-blank::before{
  content:""; }

.bp3-icon-blocked-person::before{
  content:""; }

.bp3-icon-bold::before{
  content:""; }

.bp3-icon-book::before{
  content:""; }

.bp3-icon-bookmark::before{
  content:""; }

.bp3-icon-box::before{
  content:""; }

.bp3-icon-briefcase::before{
  content:""; }

.bp3-icon-bring-data::before{
  content:""; }

.bp3-icon-build::before{
  content:""; }

.bp3-icon-calculator::before{
  content:""; }

.bp3-icon-calendar::before{
  content:""; }

.bp3-icon-camera::before{
  content:""; }

.bp3-icon-caret-down::before{
  content:"⌄"; }

.bp3-icon-caret-left::before{
  content:"〈"; }

.bp3-icon-caret-right::before{
  content:"〉"; }

.bp3-icon-caret-up::before{
  content:"⌃"; }

.bp3-icon-cell-tower::before{
  content:""; }

.bp3-icon-changes::before{
  content:""; }

.bp3-icon-chart::before{
  content:""; }

.bp3-icon-chat::before{
  content:""; }

.bp3-icon-chevron-backward::before{
  content:""; }

.bp3-icon-chevron-down::before{
  content:""; }

.bp3-icon-chevron-forward::before{
  content:""; }

.bp3-icon-chevron-left::before{
  content:""; }

.bp3-icon-chevron-right::before{
  content:""; }

.bp3-icon-chevron-up::before{
  content:""; }

.bp3-icon-circle::before{
  content:""; }

.bp3-icon-circle-arrow-down::before{
  content:""; }

.bp3-icon-circle-arrow-left::before{
  content:""; }

.bp3-icon-circle-arrow-right::before{
  content:""; }

.bp3-icon-circle-arrow-up::before{
  content:""; }

.bp3-icon-citation::before{
  content:""; }

.bp3-icon-clean::before{
  content:""; }

.bp3-icon-clipboard::before{
  content:""; }

.bp3-icon-cloud::before{
  content:"☁"; }

.bp3-icon-cloud-download::before{
  content:""; }

.bp3-icon-cloud-upload::before{
  content:""; }

.bp3-icon-code::before{
  content:""; }

.bp3-icon-code-block::before{
  content:""; }

.bp3-icon-cog::before{
  content:""; }

.bp3-icon-collapse-all::before{
  content:""; }

.bp3-icon-column-layout::before{
  content:""; }

.bp3-icon-comment::before{
  content:""; }

.bp3-icon-comparison::before{
  content:""; }

.bp3-icon-compass::before{
  content:""; }

.bp3-icon-compressed::before{
  content:""; }

.bp3-icon-confirm::before{
  content:""; }

.bp3-icon-console::before{
  content:""; }

.bp3-icon-contrast::before{
  content:""; }

.bp3-icon-control::before{
  content:""; }

.bp3-icon-credit-card::before{
  content:""; }

.bp3-icon-cross::before{
  content:"✗"; }

.bp3-icon-crown::before{
  content:""; }

.bp3-icon-cube::before{
  content:""; }

.bp3-icon-cube-add::before{
  content:""; }

.bp3-icon-cube-remove::before{
  content:""; }

.bp3-icon-curved-range-chart::before{
  content:""; }

.bp3-icon-cut::before{
  content:""; }

.bp3-icon-dashboard::before{
  content:""; }

.bp3-icon-data-lineage::before{
  content:""; }

.bp3-icon-database::before{
  content:""; }

.bp3-icon-delete::before{
  content:""; }

.bp3-icon-delta::before{
  content:"Δ"; }

.bp3-icon-derive-column::before{
  content:""; }

.bp3-icon-desktop::before{
  content:""; }

.bp3-icon-diagnosis::before{
  content:""; }

.bp3-icon-diagram-tree::before{
  content:""; }

.bp3-icon-direction-left::before{
  content:""; }

.bp3-icon-direction-right::before{
  content:""; }

.bp3-icon-disable::before{
  content:""; }

.bp3-icon-document::before{
  content:""; }

.bp3-icon-document-open::before{
  content:""; }

.bp3-icon-document-share::before{
  content:""; }

.bp3-icon-dollar::before{
  content:"$"; }

.bp3-icon-dot::before{
  content:"•"; }

.bp3-icon-double-caret-horizontal::before{
  content:""; }

.bp3-icon-double-caret-vertical::before{
  content:""; }

.bp3-icon-double-chevron-down::before{
  content:""; }

.bp3-icon-double-chevron-left::before{
  content:""; }

.bp3-icon-double-chevron-right::before{
  content:""; }

.bp3-icon-double-chevron-up::before{
  content:""; }

.bp3-icon-doughnut-chart::before{
  content:""; }

.bp3-icon-download::before{
  content:""; }

.bp3-icon-drag-handle-horizontal::before{
  content:""; }

.bp3-icon-drag-handle-vertical::before{
  content:""; }

.bp3-icon-draw::before{
  content:""; }

.bp3-icon-drive-time::before{
  content:""; }

.bp3-icon-duplicate::before{
  content:""; }

.bp3-icon-edit::before{
  content:"✎"; }

.bp3-icon-eject::before{
  content:"⏏"; }

.bp3-icon-endorsed::before{
  content:""; }

.bp3-icon-envelope::before{
  content:"✉"; }

.bp3-icon-equals::before{
  content:""; }

.bp3-icon-eraser::before{
  content:""; }

.bp3-icon-error::before{
  content:""; }

.bp3-icon-euro::before{
  content:"€"; }

.bp3-icon-exchange::before{
  content:""; }

.bp3-icon-exclude-row::before{
  content:""; }

.bp3-icon-expand-all::before{
  content:""; }

.bp3-icon-export::before{
  content:""; }

.bp3-icon-eye-off::before{
  content:""; }

.bp3-icon-eye-on::before{
  content:""; }

.bp3-icon-eye-open::before{
  content:""; }

.bp3-icon-fast-backward::before{
  content:""; }

.bp3-icon-fast-forward::before{
  content:""; }

.bp3-icon-feed::before{
  content:""; }

.bp3-icon-feed-subscribed::before{
  content:""; }

.bp3-icon-film::before{
  content:""; }

.bp3-icon-filter::before{
  content:""; }

.bp3-icon-filter-keep::before{
  content:""; }

.bp3-icon-filter-list::before{
  content:""; }

.bp3-icon-filter-open::before{
  content:""; }

.bp3-icon-filter-remove::before{
  content:""; }

.bp3-icon-flag::before{
  content:"⚑"; }

.bp3-icon-flame::before{
  content:""; }

.bp3-icon-flash::before{
  content:""; }

.bp3-icon-floppy-disk::before{
  content:""; }

.bp3-icon-flow-branch::before{
  content:""; }

.bp3-icon-flow-end::before{
  content:""; }

.bp3-icon-flow-linear::before{
  content:""; }

.bp3-icon-flow-review::before{
  content:""; }

.bp3-icon-flow-review-branch::before{
  content:""; }

.bp3-icon-flows::before{
  content:""; }

.bp3-icon-folder-close::before{
  content:""; }

.bp3-icon-folder-new::before{
  content:""; }

.bp3-icon-folder-open::before{
  content:""; }

.bp3-icon-folder-shared::before{
  content:""; }

.bp3-icon-folder-shared-open::before{
  content:""; }

.bp3-icon-follower::before{
  content:""; }

.bp3-icon-following::before{
  content:""; }

.bp3-icon-font::before{
  content:""; }

.bp3-icon-fork::before{
  content:""; }

.bp3-icon-form::before{
  content:""; }

.bp3-icon-full-circle::before{
  content:""; }

.bp3-icon-full-stacked-chart::before{
  content:""; }

.bp3-icon-fullscreen::before{
  content:""; }

.bp3-icon-function::before{
  content:""; }

.bp3-icon-gantt-chart::before{
  content:""; }

.bp3-icon-geolocation::before{
  content:""; }

.bp3-icon-geosearch::before{
  content:""; }

.bp3-icon-git-branch::before{
  content:""; }

.bp3-icon-git-commit::before{
  content:""; }

.bp3-icon-git-merge::before{
  content:""; }

.bp3-icon-git-new-branch::before{
  content:""; }

.bp3-icon-git-pull::before{
  content:""; }

.bp3-icon-git-push::before{
  content:""; }

.bp3-icon-git-repo::before{
  content:""; }

.bp3-icon-glass::before{
  content:""; }

.bp3-icon-globe::before{
  content:""; }

.bp3-icon-globe-network::before{
  content:""; }

.bp3-icon-graph::before{
  content:""; }

.bp3-icon-graph-remove::before{
  content:""; }

.bp3-icon-greater-than::before{
  content:""; }

.bp3-icon-greater-than-or-equal-to::before{
  content:""; }

.bp3-icon-grid::before{
  content:""; }

.bp3-icon-grid-view::before{
  content:""; }

.bp3-icon-group-objects::before{
  content:""; }

.bp3-icon-grouped-bar-chart::before{
  content:""; }

.bp3-icon-hand::before{
  content:""; }

.bp3-icon-hand-down::before{
  content:""; }

.bp3-icon-hand-left::before{
  content:""; }

.bp3-icon-hand-right::before{
  content:""; }

.bp3-icon-hand-up::before{
  content:""; }

.bp3-icon-header::before{
  content:""; }

.bp3-icon-header-one::before{
  content:""; }

.bp3-icon-header-two::before{
  content:""; }

.bp3-icon-headset::before{
  content:""; }

.bp3-icon-heart::before{
  content:"♥"; }

.bp3-icon-heart-broken::before{
  content:""; }

.bp3-icon-heat-grid::before{
  content:""; }

.bp3-icon-heatmap::before{
  content:""; }

.bp3-icon-help::before{
  content:"?"; }

.bp3-icon-helper-management::before{
  content:""; }

.bp3-icon-highlight::before{
  content:""; }

.bp3-icon-history::before{
  content:""; }

.bp3-icon-home::before{
  content:"⌂"; }

.bp3-icon-horizontal-bar-chart::before{
  content:""; }

.bp3-icon-horizontal-bar-chart-asc::before{
  content:""; }

.bp3-icon-horizontal-bar-chart-desc::before{
  content:""; }

.bp3-icon-horizontal-distribution::before{
  content:""; }

.bp3-icon-id-number::before{
  content:""; }

.bp3-icon-image-rotate-left::before{
  content:""; }

.bp3-icon-image-rotate-right::before{
  content:""; }

.bp3-icon-import::before{
  content:""; }

.bp3-icon-inbox::before{
  content:""; }

.bp3-icon-inbox-filtered::before{
  content:""; }

.bp3-icon-inbox-geo::before{
  content:""; }

.bp3-icon-inbox-search::before{
  content:""; }

.bp3-icon-inbox-update::before{
  content:""; }

.bp3-icon-info-sign::before{
  content:"ℹ"; }

.bp3-icon-inheritance::before{
  content:""; }

.bp3-icon-inner-join::before{
  content:""; }

.bp3-icon-insert::before{
  content:""; }

.bp3-icon-intersection::before{
  content:""; }

.bp3-icon-ip-address::before{
  content:""; }

.bp3-icon-issue::before{
  content:""; }

.bp3-icon-issue-closed::before{
  content:""; }

.bp3-icon-issue-new::before{
  content:""; }

.bp3-icon-italic::before{
  content:""; }

.bp3-icon-join-table::before{
  content:""; }

.bp3-icon-key::before{
  content:""; }

.bp3-icon-key-backspace::before{
  content:""; }

.bp3-icon-key-command::before{
  content:""; }

.bp3-icon-key-control::before{
  content:""; }

.bp3-icon-key-delete::before{
  content:""; }

.bp3-icon-key-enter::before{
  content:""; }

.bp3-icon-key-escape::before{
  content:""; }

.bp3-icon-key-option::before{
  content:""; }

.bp3-icon-key-shift::before{
  content:""; }

.bp3-icon-key-tab::before{
  content:""; }

.bp3-icon-known-vehicle::before{
  content:""; }

.bp3-icon-lab-test::before{
  content:""; }

.bp3-icon-label::before{
  content:""; }

.bp3-icon-layer::before{
  content:""; }

.bp3-icon-layers::before{
  content:""; }

.bp3-icon-layout::before{
  content:""; }

.bp3-icon-layout-auto::before{
  content:""; }

.bp3-icon-layout-balloon::before{
  content:""; }

.bp3-icon-layout-circle::before{
  content:""; }

.bp3-icon-layout-grid::before{
  content:""; }

.bp3-icon-layout-group-by::before{
  content:""; }

.bp3-icon-layout-hierarchy::before{
  content:""; }

.bp3-icon-layout-linear::before{
  content:""; }

.bp3-icon-layout-skew-grid::before{
  content:""; }

.bp3-icon-layout-sorted-clusters::before{
  content:""; }

.bp3-icon-learning::before{
  content:""; }

.bp3-icon-left-join::before{
  content:""; }

.bp3-icon-less-than::before{
  content:""; }

.bp3-icon-less-than-or-equal-to::before{
  content:""; }

.bp3-icon-lifesaver::before{
  content:""; }

.bp3-icon-lightbulb::before{
  content:""; }

.bp3-icon-link::before{
  content:""; }

.bp3-icon-list::before{
  content:"☰"; }

.bp3-icon-list-columns::before{
  content:""; }

.bp3-icon-list-detail-view::before{
  content:""; }

.bp3-icon-locate::before{
  content:""; }

.bp3-icon-lock::before{
  content:""; }

.bp3-icon-log-in::before{
  content:""; }

.bp3-icon-log-out::before{
  content:""; }

.bp3-icon-manual::before{
  content:""; }

.bp3-icon-manually-entered-data::before{
  content:""; }

.bp3-icon-map::before{
  content:""; }

.bp3-icon-map-create::before{
  content:""; }

.bp3-icon-map-marker::before{
  content:""; }

.bp3-icon-maximize::before{
  content:""; }

.bp3-icon-media::before{
  content:""; }

.bp3-icon-menu::before{
  content:""; }

.bp3-icon-menu-closed::before{
  content:""; }

.bp3-icon-menu-open::before{
  content:""; }

.bp3-icon-merge-columns::before{
  content:""; }

.bp3-icon-merge-links::before{
  content:""; }

.bp3-icon-minimize::before{
  content:""; }

.bp3-icon-minus::before{
  content:"−"; }

.bp3-icon-mobile-phone::before{
  content:""; }

.bp3-icon-mobile-video::before{
  content:""; }

.bp3-icon-moon::before{
  content:""; }

.bp3-icon-more::before{
  content:""; }

.bp3-icon-mountain::before{
  content:""; }

.bp3-icon-move::before{
  content:""; }

.bp3-icon-mugshot::before{
  content:""; }

.bp3-icon-multi-select::before{
  content:""; }

.bp3-icon-music::before{
  content:""; }

.bp3-icon-new-drawing::before{
  content:""; }

.bp3-icon-new-grid-item::before{
  content:""; }

.bp3-icon-new-layer::before{
  content:""; }

.bp3-icon-new-layers::before{
  content:""; }

.bp3-icon-new-link::before{
  content:""; }

.bp3-icon-new-object::before{
  content:""; }

.bp3-icon-new-person::before{
  content:""; }

.bp3-icon-new-prescription::before{
  content:""; }

.bp3-icon-new-text-box::before{
  content:""; }

.bp3-icon-ninja::before{
  content:""; }

.bp3-icon-not-equal-to::before{
  content:""; }

.bp3-icon-notifications::before{
  content:""; }

.bp3-icon-notifications-updated::before{
  content:""; }

.bp3-icon-numbered-list::before{
  content:""; }

.bp3-icon-numerical::before{
  content:""; }

.bp3-icon-office::before{
  content:""; }

.bp3-icon-offline::before{
  content:""; }

.bp3-icon-oil-field::before{
  content:""; }

.bp3-icon-one-column::before{
  content:""; }

.bp3-icon-outdated::before{
  content:""; }

.bp3-icon-page-layout::before{
  content:""; }

.bp3-icon-panel-stats::before{
  content:""; }

.bp3-icon-panel-table::before{
  content:""; }

.bp3-icon-paperclip::before{
  content:""; }

.bp3-icon-paragraph::before{
  content:""; }

.bp3-icon-path::before{
  content:""; }

.bp3-icon-path-search::before{
  content:""; }

.bp3-icon-pause::before{
  content:""; }

.bp3-icon-people::before{
  content:""; }

.bp3-icon-percentage::before{
  content:""; }

.bp3-icon-person::before{
  content:""; }

.bp3-icon-phone::before{
  content:"☎"; }

.bp3-icon-pie-chart::before{
  content:""; }

.bp3-icon-pin::before{
  content:""; }

.bp3-icon-pivot::before{
  content:""; }

.bp3-icon-pivot-table::before{
  content:""; }

.bp3-icon-play::before{
  content:""; }

.bp3-icon-plus::before{
  content:"+"; }

.bp3-icon-polygon-filter::before{
  content:""; }

.bp3-icon-power::before{
  content:""; }

.bp3-icon-predictive-analysis::before{
  content:""; }

.bp3-icon-prescription::before{
  content:""; }

.bp3-icon-presentation::before{
  content:""; }

.bp3-icon-print::before{
  content:"⎙"; }

.bp3-icon-projects::before{
  content:""; }

.bp3-icon-properties::before{
  content:""; }

.bp3-icon-property::before{
  content:""; }

.bp3-icon-publish-function::before{
  content:""; }

.bp3-icon-pulse::before{
  content:""; }

.bp3-icon-random::before{
  content:""; }

.bp3-icon-record::before{
  content:""; }

.bp3-icon-redo::before{
  content:""; }

.bp3-icon-refresh::before{
  content:""; }

.bp3-icon-regression-chart::before{
  content:""; }

.bp3-icon-remove::before{
  content:""; }

.bp3-icon-remove-column::before{
  content:""; }

.bp3-icon-remove-column-left::before{
  content:""; }

.bp3-icon-remove-column-right::before{
  content:""; }

.bp3-icon-remove-row-bottom::before{
  content:""; }

.bp3-icon-remove-row-top::before{
  content:""; }

.bp3-icon-repeat::before{
  content:""; }

.bp3-icon-reset::before{
  content:""; }

.bp3-icon-resolve::before{
  content:""; }

.bp3-icon-rig::before{
  content:""; }

.bp3-icon-right-join::before{
  content:""; }

.bp3-icon-ring::before{
  content:""; }

.bp3-icon-rotate-document::before{
  content:""; }

.bp3-icon-rotate-page::before{
  content:""; }

.bp3-icon-satellite::before{
  content:""; }

.bp3-icon-saved::before{
  content:""; }

.bp3-icon-scatter-plot::before{
  content:""; }

.bp3-icon-search::before{
  content:""; }

.bp3-icon-search-around::before{
  content:""; }

.bp3-icon-search-template::before{
  content:""; }

.bp3-icon-search-text::before{
  content:""; }

.bp3-icon-segmented-control::before{
  content:""; }

.bp3-icon-select::before{
  content:""; }

.bp3-icon-selection::before{
  content:"⦿"; }

.bp3-icon-send-to::before{
  content:""; }

.bp3-icon-send-to-graph::before{
  content:""; }

.bp3-icon-send-to-map::before{
  content:""; }

.bp3-icon-series-add::before{
  content:""; }

.bp3-icon-series-configuration::before{
  content:""; }

.bp3-icon-series-derived::before{
  content:""; }

.bp3-icon-series-filtered::before{
  content:""; }

.bp3-icon-series-search::before{
  content:""; }

.bp3-icon-settings::before{
  content:""; }

.bp3-icon-share::before{
  content:""; }

.bp3-icon-shield::before{
  content:""; }

.bp3-icon-shop::before{
  content:""; }

.bp3-icon-shopping-cart::before{
  content:""; }

.bp3-icon-signal-search::before{
  content:""; }

.bp3-icon-sim-card::before{
  content:""; }

.bp3-icon-slash::before{
  content:""; }

.bp3-icon-small-cross::before{
  content:""; }

.bp3-icon-small-minus::before{
  content:""; }

.bp3-icon-small-plus::before{
  content:""; }

.bp3-icon-small-tick::before{
  content:""; }

.bp3-icon-snowflake::before{
  content:""; }

.bp3-icon-social-media::before{
  content:""; }

.bp3-icon-sort::before{
  content:""; }

.bp3-icon-sort-alphabetical::before{
  content:""; }

.bp3-icon-sort-alphabetical-desc::before{
  content:""; }

.bp3-icon-sort-asc::before{
  content:""; }

.bp3-icon-sort-desc::before{
  content:""; }

.bp3-icon-sort-numerical::before{
  content:""; }

.bp3-icon-sort-numerical-desc::before{
  content:""; }

.bp3-icon-split-columns::before{
  content:""; }

.bp3-icon-square::before{
  content:""; }

.bp3-icon-stacked-chart::before{
  content:""; }

.bp3-icon-star::before{
  content:"★"; }

.bp3-icon-star-empty::before{
  content:"☆"; }

.bp3-icon-step-backward::before{
  content:""; }

.bp3-icon-step-chart::before{
  content:""; }

.bp3-icon-step-forward::before{
  content:""; }

.bp3-icon-stop::before{
  content:""; }

.bp3-icon-stopwatch::before{
  content:""; }

.bp3-icon-strikethrough::before{
  content:""; }

.bp3-icon-style::before{
  content:""; }

.bp3-icon-swap-horizontal::before{
  content:""; }

.bp3-icon-swap-vertical::before{
  content:""; }

.bp3-icon-symbol-circle::before{
  content:""; }

.bp3-icon-symbol-cross::before{
  content:""; }

.bp3-icon-symbol-diamond::before{
  content:""; }

.bp3-icon-symbol-square::before{
  content:""; }

.bp3-icon-symbol-triangle-down::before{
  content:""; }

.bp3-icon-symbol-triangle-up::before{
  content:""; }

.bp3-icon-tag::before{
  content:""; }

.bp3-icon-take-action::before{
  content:""; }

.bp3-icon-taxi::before{
  content:""; }

.bp3-icon-text-highlight::before{
  content:""; }

.bp3-icon-th::before{
  content:""; }

.bp3-icon-th-derived::before{
  content:""; }

.bp3-icon-th-disconnect::before{
  content:""; }

.bp3-icon-th-filtered::before{
  content:""; }

.bp3-icon-th-list::before{
  content:""; }

.bp3-icon-thumbs-down::before{
  content:""; }

.bp3-icon-thumbs-up::before{
  content:""; }

.bp3-icon-tick::before{
  content:"✓"; }

.bp3-icon-tick-circle::before{
  content:""; }

.bp3-icon-time::before{
  content:"⏲"; }

.bp3-icon-timeline-area-chart::before{
  content:""; }

.bp3-icon-timeline-bar-chart::before{
  content:""; }

.bp3-icon-timeline-events::before{
  content:""; }

.bp3-icon-timeline-line-chart::before{
  content:""; }

.bp3-icon-tint::before{
  content:""; }

.bp3-icon-torch::before{
  content:""; }

.bp3-icon-tractor::before{
  content:""; }

.bp3-icon-train::before{
  content:""; }

.bp3-icon-translate::before{
  content:""; }

.bp3-icon-trash::before{
  content:""; }

.bp3-icon-tree::before{
  content:""; }

.bp3-icon-trending-down::before{
  content:""; }

.bp3-icon-trending-up::before{
  content:""; }

.bp3-icon-truck::before{
  content:""; }

.bp3-icon-two-columns::before{
  content:""; }

.bp3-icon-unarchive::before{
  content:""; }

.bp3-icon-underline::before{
  content:"⎁"; }

.bp3-icon-undo::before{
  content:"⎌"; }

.bp3-icon-ungroup-objects::before{
  content:""; }

.bp3-icon-unknown-vehicle::before{
  content:""; }

.bp3-icon-unlock::before{
  content:""; }

.bp3-icon-unpin::before{
  content:""; }

.bp3-icon-unresolve::before{
  content:""; }

.bp3-icon-updated::before{
  content:""; }

.bp3-icon-upload::before{
  content:""; }

.bp3-icon-user::before{
  content:""; }

.bp3-icon-variable::before{
  content:""; }

.bp3-icon-vertical-bar-chart-asc::before{
  content:""; }

.bp3-icon-vertical-bar-chart-desc::before{
  content:""; }

.bp3-icon-vertical-distribution::before{
  content:""; }

.bp3-icon-video::before{
  content:""; }

.bp3-icon-volume-down::before{
  content:""; }

.bp3-icon-volume-off::before{
  content:""; }

.bp3-icon-volume-up::before{
  content:""; }

.bp3-icon-walk::before{
  content:""; }

.bp3-icon-warning-sign::before{
  content:""; }

.bp3-icon-waterfall-chart::before{
  content:""; }

.bp3-icon-widget::before{
  content:""; }

.bp3-icon-widget-button::before{
  content:""; }

.bp3-icon-widget-footer::before{
  content:""; }

.bp3-icon-widget-header::before{
  content:""; }

.bp3-icon-wrench::before{
  content:""; }

.bp3-icon-zoom-in::before{
  content:""; }

.bp3-icon-zoom-out::before{
  content:""; }

.bp3-icon-zoom-to-fit::before{
  content:""; }
.bp3-submenu > .bp3-popover-wrapper{
  display:block; }

.bp3-submenu .bp3-popover-target{
  display:block; }
  .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-menu-item{ }

.bp3-submenu.bp3-popover{
  -webkit-box-shadow:none;
          box-shadow:none;
  padding:0 5px; }
  .bp3-submenu.bp3-popover > .bp3-popover-content{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-submenu.bp3-popover, .bp3-submenu.bp3-popover.bp3-dark{
    -webkit-box-shadow:none;
            box-shadow:none; }
    .bp3-dark .bp3-submenu.bp3-popover > .bp3-popover-content, .bp3-submenu.bp3-popover.bp3-dark > .bp3-popover-content{
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }
.bp3-menu{
  background:#ffffff;
  border-radius:3px;
  color:#182026;
  list-style:none;
  margin:0;
  min-width:180px;
  padding:5px;
  text-align:left; }

.bp3-menu-divider{
  border-top:1px solid rgba(16, 22, 26, 0.15);
  display:block;
  margin:5px; }
  .bp3-dark .bp3-menu-divider{
    border-top-color:rgba(255, 255, 255, 0.15); }

.bp3-menu-item{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:start;
      -ms-flex-align:start;
          align-items:flex-start;
  border-radius:2px;
  color:inherit;
  line-height:20px;
  padding:5px 7px;
  text-decoration:none;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-menu-item > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-menu-item > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-menu-item::before,
  .bp3-menu-item > *{
    margin-right:7px; }
  .bp3-menu-item:empty::before,
  .bp3-menu-item > :last-child{
    margin-right:0; }
  .bp3-menu-item > .bp3-fill{
    word-break:break-word; }
  .bp3-menu-item:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-menu-item{
    background-color:rgba(167, 182, 194, 0.3);
    cursor:pointer;
    text-decoration:none; }
  .bp3-menu-item.bp3-disabled{
    background-color:inherit;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed; }
  .bp3-dark .bp3-menu-item{
    color:inherit; }
    .bp3-dark .bp3-menu-item:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-menu-item{
      background-color:rgba(138, 155, 168, 0.15);
      color:inherit; }
    .bp3-dark .bp3-menu-item.bp3-disabled{
      background-color:inherit;
      color:rgba(167, 182, 194, 0.6); }
  .bp3-menu-item.bp3-intent-primary{
    color:#106ba3; }
    .bp3-menu-item.bp3-intent-primary .bp3-icon{
      color:inherit; }
    .bp3-menu-item.bp3-intent-primary::before, .bp3-menu-item.bp3-intent-primary::after,
    .bp3-menu-item.bp3-intent-primary .bp3-menu-item-label{
      color:#106ba3; }
    .bp3-menu-item.bp3-intent-primary:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-menu-item.bp3-intent-primary.bp3-active{
      background-color:#137cbd; }
    .bp3-menu-item.bp3-intent-primary:active{
      background-color:#106ba3; }
    .bp3-menu-item.bp3-intent-primary:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-menu-item.bp3-intent-primary:hover::before, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::before, .bp3-menu-item.bp3-intent-primary:hover::after, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::after,
    .bp3-menu-item.bp3-intent-primary:hover .bp3-menu-item-label,
    .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item .bp3-menu-item-label, .bp3-menu-item.bp3-intent-primary:active, .bp3-menu-item.bp3-intent-primary:active::before, .bp3-menu-item.bp3-intent-primary:active::after,
    .bp3-menu-item.bp3-intent-primary:active .bp3-menu-item-label, .bp3-menu-item.bp3-intent-primary.bp3-active, .bp3-menu-item.bp3-intent-primary.bp3-active::before, .bp3-menu-item.bp3-intent-primary.bp3-active::after,
    .bp3-menu-item.bp3-intent-primary.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-menu-item.bp3-intent-success{
    color:#0d8050; }
    .bp3-menu-item.bp3-intent-success .bp3-icon{
      color:inherit; }
    .bp3-menu-item.bp3-intent-success::before, .bp3-menu-item.bp3-intent-success::after,
    .bp3-menu-item.bp3-intent-success .bp3-menu-item-label{
      color:#0d8050; }
    .bp3-menu-item.bp3-intent-success:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-menu-item.bp3-intent-success.bp3-active{
      background-color:#0f9960; }
    .bp3-menu-item.bp3-intent-success:active{
      background-color:#0d8050; }
    .bp3-menu-item.bp3-intent-success:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-menu-item.bp3-intent-success:hover::before, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::before, .bp3-menu-item.bp3-intent-success:hover::after, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::after,
    .bp3-menu-item.bp3-intent-success:hover .bp3-menu-item-label,
    .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item .bp3-menu-item-label, .bp3-menu-item.bp3-intent-success:active, .bp3-menu-item.bp3-intent-success:active::before, .bp3-menu-item.bp3-intent-success:active::after,
    .bp3-menu-item.bp3-intent-success:active .bp3-menu-item-label, .bp3-menu-item.bp3-intent-success.bp3-active, .bp3-menu-item.bp3-intent-success.bp3-active::before, .bp3-menu-item.bp3-intent-success.bp3-active::after,
    .bp3-menu-item.bp3-intent-success.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-menu-item.bp3-intent-warning{
    color:#bf7326; }
    .bp3-menu-item.bp3-intent-warning .bp3-icon{
      color:inherit; }
    .bp3-menu-item.bp3-intent-warning::before, .bp3-menu-item.bp3-intent-warning::after,
    .bp3-menu-item.bp3-intent-warning .bp3-menu-item-label{
      color:#bf7326; }
    .bp3-menu-item.bp3-intent-warning:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-menu-item.bp3-intent-warning.bp3-active{
      background-color:#d9822b; }
    .bp3-menu-item.bp3-intent-warning:active{
      background-color:#bf7326; }
    .bp3-menu-item.bp3-intent-warning:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-menu-item.bp3-intent-warning:hover::before, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::before, .bp3-menu-item.bp3-intent-warning:hover::after, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::after,
    .bp3-menu-item.bp3-intent-warning:hover .bp3-menu-item-label,
    .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item .bp3-menu-item-label, .bp3-menu-item.bp3-intent-warning:active, .bp3-menu-item.bp3-intent-warning:active::before, .bp3-menu-item.bp3-intent-warning:active::after,
    .bp3-menu-item.bp3-intent-warning:active .bp3-menu-item-label, .bp3-menu-item.bp3-intent-warning.bp3-active, .bp3-menu-item.bp3-intent-warning.bp3-active::before, .bp3-menu-item.bp3-intent-warning.bp3-active::after,
    .bp3-menu-item.bp3-intent-warning.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-menu-item.bp3-intent-danger{
    color:#c23030; }
    .bp3-menu-item.bp3-intent-danger .bp3-icon{
      color:inherit; }
    .bp3-menu-item.bp3-intent-danger::before, .bp3-menu-item.bp3-intent-danger::after,
    .bp3-menu-item.bp3-intent-danger .bp3-menu-item-label{
      color:#c23030; }
    .bp3-menu-item.bp3-intent-danger:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-menu-item.bp3-intent-danger.bp3-active{
      background-color:#db3737; }
    .bp3-menu-item.bp3-intent-danger:active{
      background-color:#c23030; }
    .bp3-menu-item.bp3-intent-danger:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-menu-item.bp3-intent-danger:hover::before, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::before, .bp3-menu-item.bp3-intent-danger:hover::after, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::after,
    .bp3-menu-item.bp3-intent-danger:hover .bp3-menu-item-label,
    .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item .bp3-menu-item-label, .bp3-menu-item.bp3-intent-danger:active, .bp3-menu-item.bp3-intent-danger:active::before, .bp3-menu-item.bp3-intent-danger:active::after,
    .bp3-menu-item.bp3-intent-danger:active .bp3-menu-item-label, .bp3-menu-item.bp3-intent-danger.bp3-active, .bp3-menu-item.bp3-intent-danger.bp3-active::before, .bp3-menu-item.bp3-intent-danger.bp3-active::after,
    .bp3-menu-item.bp3-intent-danger.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-menu-item::before{
    font-family:"Icons16", sans-serif;
    font-size:16px;
    font-style:normal;
    font-weight:400;
    line-height:1;
    -moz-osx-font-smoothing:grayscale;
    -webkit-font-smoothing:antialiased;
    margin-right:7px; }
  .bp3-menu-item::before,
  .bp3-menu-item > .bp3-icon{
    color:#5c7080;
    margin-top:2px; }
  .bp3-menu-item .bp3-menu-item-label{
    color:#5c7080; }
  .bp3-menu-item:hover, .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-menu-item{
    color:inherit; }
  .bp3-menu-item.bp3-active, .bp3-menu-item:active{
    background-color:rgba(115, 134, 148, 0.3); }
  .bp3-menu-item.bp3-disabled{
    background-color:inherit !important;
    color:rgba(92, 112, 128, 0.6) !important;
    cursor:not-allowed !important;
    outline:none !important; }
    .bp3-menu-item.bp3-disabled::before,
    .bp3-menu-item.bp3-disabled > .bp3-icon,
    .bp3-menu-item.bp3-disabled .bp3-menu-item-label{
      color:rgba(92, 112, 128, 0.6) !important; }
  .bp3-large .bp3-menu-item{
    font-size:16px;
    line-height:22px;
    padding:9px 7px; }
    .bp3-large .bp3-menu-item .bp3-icon{
      margin-top:3px; }
    .bp3-large .bp3-menu-item::before{
      font-family:"Icons20", sans-serif;
      font-size:20px;
      font-style:normal;
      font-weight:400;
      line-height:1;
      -moz-osx-font-smoothing:grayscale;
      -webkit-font-smoothing:antialiased;
      margin-right:10px;
      margin-top:1px; }

button.bp3-menu-item{
  background:none;
  border:none;
  text-align:left;
  width:100%; }
.bp3-menu-header{
  border-top:1px solid rgba(16, 22, 26, 0.15);
  display:block;
  margin:5px;
  cursor:default;
  padding-left:2px; }
  .bp3-dark .bp3-menu-header{
    border-top-color:rgba(255, 255, 255, 0.15); }
  .bp3-menu-header:first-of-type{
    border-top:none; }
  .bp3-menu-header > h6{
    color:#182026;
    font-weight:600;
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
    word-wrap:normal;
    line-height:17px;
    margin:0;
    padding:10px 7px 0 1px; }
    .bp3-dark .bp3-menu-header > h6{
      color:#f5f8fa; }
  .bp3-menu-header:first-of-type > h6{
    padding-top:0; }
  .bp3-large .bp3-menu-header > h6{
    font-size:18px;
    padding-bottom:5px;
    padding-top:15px; }
  .bp3-large .bp3-menu-header:first-of-type > h6{
    padding-top:0; }

.bp3-dark .bp3-menu{
  background:#30404d;
  color:#f5f8fa; }

.bp3-dark .bp3-menu-item{ }
  .bp3-dark .bp3-menu-item.bp3-intent-primary{
    color:#48aff0; }
    .bp3-dark .bp3-menu-item.bp3-intent-primary .bp3-icon{
      color:inherit; }
    .bp3-dark .bp3-menu-item.bp3-intent-primary::before, .bp3-dark .bp3-menu-item.bp3-intent-primary::after,
    .bp3-dark .bp3-menu-item.bp3-intent-primary .bp3-menu-item-label{
      color:#48aff0; }
    .bp3-dark .bp3-menu-item.bp3-intent-primary:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-primary.bp3-active{
      background-color:#137cbd; }
    .bp3-dark .bp3-menu-item.bp3-intent-primary:active{
      background-color:#106ba3; }
    .bp3-dark .bp3-menu-item.bp3-intent-primary:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-primary:hover::before, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::before, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::before, .bp3-dark .bp3-menu-item.bp3-intent-primary:hover::after, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::after, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item::after,
    .bp3-dark .bp3-menu-item.bp3-intent-primary:hover .bp3-menu-item-label,
    .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item .bp3-menu-item-label,
    .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-primary.bp3-menu-item .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-primary:active, .bp3-dark .bp3-menu-item.bp3-intent-primary:active::before, .bp3-dark .bp3-menu-item.bp3-intent-primary:active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-primary:active .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-primary.bp3-active, .bp3-dark .bp3-menu-item.bp3-intent-primary.bp3-active::before, .bp3-dark .bp3-menu-item.bp3-intent-primary.bp3-active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-primary.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-dark .bp3-menu-item.bp3-intent-success{
    color:#3dcc91; }
    .bp3-dark .bp3-menu-item.bp3-intent-success .bp3-icon{
      color:inherit; }
    .bp3-dark .bp3-menu-item.bp3-intent-success::before, .bp3-dark .bp3-menu-item.bp3-intent-success::after,
    .bp3-dark .bp3-menu-item.bp3-intent-success .bp3-menu-item-label{
      color:#3dcc91; }
    .bp3-dark .bp3-menu-item.bp3-intent-success:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-success.bp3-active{
      background-color:#0f9960; }
    .bp3-dark .bp3-menu-item.bp3-intent-success:active{
      background-color:#0d8050; }
    .bp3-dark .bp3-menu-item.bp3-intent-success:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-success:hover::before, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::before, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::before, .bp3-dark .bp3-menu-item.bp3-intent-success:hover::after, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::after, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item::after,
    .bp3-dark .bp3-menu-item.bp3-intent-success:hover .bp3-menu-item-label,
    .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item .bp3-menu-item-label,
    .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-success.bp3-menu-item .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-success:active, .bp3-dark .bp3-menu-item.bp3-intent-success:active::before, .bp3-dark .bp3-menu-item.bp3-intent-success:active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-success:active .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-success.bp3-active, .bp3-dark .bp3-menu-item.bp3-intent-success.bp3-active::before, .bp3-dark .bp3-menu-item.bp3-intent-success.bp3-active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-success.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-dark .bp3-menu-item.bp3-intent-warning{
    color:#ffb366; }
    .bp3-dark .bp3-menu-item.bp3-intent-warning .bp3-icon{
      color:inherit; }
    .bp3-dark .bp3-menu-item.bp3-intent-warning::before, .bp3-dark .bp3-menu-item.bp3-intent-warning::after,
    .bp3-dark .bp3-menu-item.bp3-intent-warning .bp3-menu-item-label{
      color:#ffb366; }
    .bp3-dark .bp3-menu-item.bp3-intent-warning:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-warning.bp3-active{
      background-color:#d9822b; }
    .bp3-dark .bp3-menu-item.bp3-intent-warning:active{
      background-color:#bf7326; }
    .bp3-dark .bp3-menu-item.bp3-intent-warning:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-warning:hover::before, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::before, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::before, .bp3-dark .bp3-menu-item.bp3-intent-warning:hover::after, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::after, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item::after,
    .bp3-dark .bp3-menu-item.bp3-intent-warning:hover .bp3-menu-item-label,
    .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item .bp3-menu-item-label,
    .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-warning.bp3-menu-item .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-warning:active, .bp3-dark .bp3-menu-item.bp3-intent-warning:active::before, .bp3-dark .bp3-menu-item.bp3-intent-warning:active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-warning:active .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-warning.bp3-active, .bp3-dark .bp3-menu-item.bp3-intent-warning.bp3-active::before, .bp3-dark .bp3-menu-item.bp3-intent-warning.bp3-active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-warning.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-dark .bp3-menu-item.bp3-intent-danger{
    color:#ff7373; }
    .bp3-dark .bp3-menu-item.bp3-intent-danger .bp3-icon{
      color:inherit; }
    .bp3-dark .bp3-menu-item.bp3-intent-danger::before, .bp3-dark .bp3-menu-item.bp3-intent-danger::after,
    .bp3-dark .bp3-menu-item.bp3-intent-danger .bp3-menu-item-label{
      color:#ff7373; }
    .bp3-dark .bp3-menu-item.bp3-intent-danger:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-danger.bp3-active{
      background-color:#db3737; }
    .bp3-dark .bp3-menu-item.bp3-intent-danger:active{
      background-color:#c23030; }
    .bp3-dark .bp3-menu-item.bp3-intent-danger:hover, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item, .bp3-dark .bp3-menu-item.bp3-intent-danger:hover::before, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::before, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::before, .bp3-dark .bp3-menu-item.bp3-intent-danger:hover::after, .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::after, .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item::after,
    .bp3-dark .bp3-menu-item.bp3-intent-danger:hover .bp3-menu-item-label,
    .bp3-dark .bp3-submenu .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item .bp3-menu-item-label,
    .bp3-submenu .bp3-dark .bp3-popover-target.bp3-popover-open > .bp3-intent-danger.bp3-menu-item .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-danger:active, .bp3-dark .bp3-menu-item.bp3-intent-danger:active::before, .bp3-dark .bp3-menu-item.bp3-intent-danger:active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-danger:active .bp3-menu-item-label, .bp3-dark .bp3-menu-item.bp3-intent-danger.bp3-active, .bp3-dark .bp3-menu-item.bp3-intent-danger.bp3-active::before, .bp3-dark .bp3-menu-item.bp3-intent-danger.bp3-active::after,
    .bp3-dark .bp3-menu-item.bp3-intent-danger.bp3-active .bp3-menu-item-label{
      color:#ffffff; }
  .bp3-dark .bp3-menu-item::before,
  .bp3-dark .bp3-menu-item > .bp3-icon{
    color:#a7b6c2; }
  .bp3-dark .bp3-menu-item .bp3-menu-item-label{
    color:#a7b6c2; }
  .bp3-dark .bp3-menu-item.bp3-active, .bp3-dark .bp3-menu-item:active{
    background-color:rgba(138, 155, 168, 0.3); }
  .bp3-dark .bp3-menu-item.bp3-disabled{
    color:rgba(167, 182, 194, 0.6) !important; }
    .bp3-dark .bp3-menu-item.bp3-disabled::before,
    .bp3-dark .bp3-menu-item.bp3-disabled > .bp3-icon,
    .bp3-dark .bp3-menu-item.bp3-disabled .bp3-menu-item-label{
      color:rgba(167, 182, 194, 0.6) !important; }

.bp3-dark .bp3-menu-divider,
.bp3-dark .bp3-menu-header{
  border-color:rgba(255, 255, 255, 0.15); }

.bp3-dark .bp3-menu-header > h6{
  color:#f5f8fa; }

.bp3-label .bp3-menu{
  margin-top:5px; }
.bp3-navbar{
  background-color:#ffffff;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.2);
  height:50px;
  padding:0 15px;
  position:relative;
  width:100%;
  z-index:10; }
  .bp3-navbar.bp3-dark,
  .bp3-dark .bp3-navbar{
    background-color:#394b59; }
  .bp3-navbar.bp3-dark{
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-dark .bp3-navbar{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 0 0 rgba(16, 22, 26, 0), 0 1px 1px rgba(16, 22, 26, 0.4); }
  .bp3-navbar.bp3-fixed-top{
    left:0;
    position:fixed;
    right:0;
    top:0; }

.bp3-navbar-heading{
  font-size:16px;
  margin-right:15px; }

.bp3-navbar-group{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  height:50px; }
  .bp3-navbar-group.bp3-align-left{
    float:left; }
  .bp3-navbar-group.bp3-align-right{
    float:right; }

.bp3-navbar-divider{
  border-left:1px solid rgba(16, 22, 26, 0.15);
  height:20px;
  margin:0 10px; }
  .bp3-dark .bp3-navbar-divider{
    border-left-color:rgba(255, 255, 255, 0.15); }
.bp3-non-ideal-state{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  height:100%;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  text-align:center;
  width:100%; }
  .bp3-non-ideal-state > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-non-ideal-state > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-non-ideal-state::before,
  .bp3-non-ideal-state > *{
    margin-bottom:20px; }
  .bp3-non-ideal-state:empty::before,
  .bp3-non-ideal-state > :last-child{
    margin-bottom:0; }
  .bp3-non-ideal-state > *{
    max-width:400px; }

.bp3-non-ideal-state-visual{
  color:rgba(92, 112, 128, 0.6);
  font-size:60px; }
  .bp3-dark .bp3-non-ideal-state-visual{
    color:rgba(167, 182, 194, 0.6); }

.bp3-overflow-list{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -ms-flex-wrap:nowrap;
      flex-wrap:nowrap;
  min-width:0; }

.bp3-overflow-list-spacer{
  -ms-flex-negative:1;
      flex-shrink:1;
  width:1px; }

body.bp3-overlay-open{
  overflow:hidden; }

.bp3-overlay{
  bottom:0;
  left:0;
  position:static;
  right:0;
  top:0;
  z-index:20; }
  .bp3-overlay:not(.bp3-overlay-open){
    pointer-events:none; }
  .bp3-overlay.bp3-overlay-container{
    overflow:hidden;
    position:fixed; }
    .bp3-overlay.bp3-overlay-container.bp3-overlay-inline{
      position:absolute; }
  .bp3-overlay.bp3-overlay-scroll-container{
    overflow:auto;
    position:fixed; }
    .bp3-overlay.bp3-overlay-scroll-container.bp3-overlay-inline{
      position:absolute; }
  .bp3-overlay.bp3-overlay-inline{
    display:inline;
    overflow:visible; }

.bp3-overlay-content{
  position:fixed;
  z-index:20; }
  .bp3-overlay-inline .bp3-overlay-content,
  .bp3-overlay-scroll-container .bp3-overlay-content{
    position:absolute; }

.bp3-overlay-backdrop{
  bottom:0;
  left:0;
  position:fixed;
  right:0;
  top:0;
  opacity:1;
  background-color:rgba(16, 22, 26, 0.7);
  overflow:auto;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none;
  z-index:20; }
  .bp3-overlay-backdrop.bp3-overlay-enter, .bp3-overlay-backdrop.bp3-overlay-appear{
    opacity:0; }
  .bp3-overlay-backdrop.bp3-overlay-enter-active, .bp3-overlay-backdrop.bp3-overlay-appear-active{
    opacity:1;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:200ms;
            transition-duration:200ms;
    -webkit-transition-property:opacity;
    transition-property:opacity;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-overlay-backdrop.bp3-overlay-exit{
    opacity:1; }
  .bp3-overlay-backdrop.bp3-overlay-exit-active{
    opacity:0;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:200ms;
            transition-duration:200ms;
    -webkit-transition-property:opacity;
    transition-property:opacity;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-overlay-backdrop:focus{
    outline:none; }
  .bp3-overlay-inline .bp3-overlay-backdrop{
    position:absolute; }
.bp3-panel-stack{
  overflow:hidden;
  position:relative; }

.bp3-panel-stack-header{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  -webkit-box-shadow:0 1px rgba(16, 22, 26, 0.15);
          box-shadow:0 1px rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -ms-flex-negative:0;
      flex-shrink:0;
  height:30px;
  z-index:1; }
  .bp3-dark .bp3-panel-stack-header{
    -webkit-box-shadow:0 1px rgba(255, 255, 255, 0.15);
            box-shadow:0 1px rgba(255, 255, 255, 0.15); }
  .bp3-panel-stack-header > span{
    -webkit-box-align:stretch;
        -ms-flex-align:stretch;
            align-items:stretch;
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    -webkit-box-flex:1;
        -ms-flex:1;
            flex:1; }
  .bp3-panel-stack-header .bp3-heading{
    margin:0 5px; }

.bp3-button.bp3-panel-stack-header-back{
  margin-left:5px;
  padding-left:0;
  white-space:nowrap; }
  .bp3-button.bp3-panel-stack-header-back .bp3-icon{
    margin:0 2px; }

.bp3-panel-stack-view{
  bottom:0;
  left:0;
  position:absolute;
  right:0;
  top:0;
  background-color:#ffffff;
  border-right:1px solid rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  margin-right:-1px;
  overflow-y:auto;
  z-index:1; }
  .bp3-dark .bp3-panel-stack-view{
    background-color:#30404d; }
  .bp3-panel-stack-view:nth-last-child(n + 4){
    display:none; }

.bp3-panel-stack-push .bp3-panel-stack-enter, .bp3-panel-stack-push .bp3-panel-stack-appear{
  -webkit-transform:translateX(100%);
          transform:translateX(100%);
  opacity:0; }

.bp3-panel-stack-push .bp3-panel-stack-enter-active, .bp3-panel-stack-push .bp3-panel-stack-appear-active{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack-push .bp3-panel-stack-exit{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1; }

.bp3-panel-stack-push .bp3-panel-stack-exit-active{
  -webkit-transform:translateX(-50%);
          transform:translateX(-50%);
  opacity:0;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack-pop .bp3-panel-stack-enter, .bp3-panel-stack-pop .bp3-panel-stack-appear{
  -webkit-transform:translateX(-50%);
          transform:translateX(-50%);
  opacity:0; }

.bp3-panel-stack-pop .bp3-panel-stack-enter-active, .bp3-panel-stack-pop .bp3-panel-stack-appear-active{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack-pop .bp3-panel-stack-exit{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1; }

.bp3-panel-stack-pop .bp3-panel-stack-exit-active{
  -webkit-transform:translateX(100%);
          transform:translateX(100%);
  opacity:0;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }
.bp3-panel-stack2{
  overflow:hidden;
  position:relative; }

.bp3-panel-stack2-header{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  -webkit-box-shadow:0 1px rgba(16, 22, 26, 0.15);
          box-shadow:0 1px rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -ms-flex-negative:0;
      flex-shrink:0;
  height:30px;
  z-index:1; }
  .bp3-dark .bp3-panel-stack2-header{
    -webkit-box-shadow:0 1px rgba(255, 255, 255, 0.15);
            box-shadow:0 1px rgba(255, 255, 255, 0.15); }
  .bp3-panel-stack2-header > span{
    -webkit-box-align:stretch;
        -ms-flex-align:stretch;
            align-items:stretch;
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    -webkit-box-flex:1;
        -ms-flex:1;
            flex:1; }
  .bp3-panel-stack2-header .bp3-heading{
    margin:0 5px; }

.bp3-button.bp3-panel-stack2-header-back{
  margin-left:5px;
  padding-left:0;
  white-space:nowrap; }
  .bp3-button.bp3-panel-stack2-header-back .bp3-icon{
    margin:0 2px; }

.bp3-panel-stack2-view{
  bottom:0;
  left:0;
  position:absolute;
  right:0;
  top:0;
  background-color:#ffffff;
  border-right:1px solid rgba(16, 22, 26, 0.15);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  margin-right:-1px;
  overflow-y:auto;
  z-index:1; }
  .bp3-dark .bp3-panel-stack2-view{
    background-color:#30404d; }
  .bp3-panel-stack2-view:nth-last-child(n + 4){
    display:none; }

.bp3-panel-stack2-push .bp3-panel-stack2-enter, .bp3-panel-stack2-push .bp3-panel-stack2-appear{
  -webkit-transform:translateX(100%);
          transform:translateX(100%);
  opacity:0; }

.bp3-panel-stack2-push .bp3-panel-stack2-enter-active, .bp3-panel-stack2-push .bp3-panel-stack2-appear-active{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack2-push .bp3-panel-stack2-exit{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1; }

.bp3-panel-stack2-push .bp3-panel-stack2-exit-active{
  -webkit-transform:translateX(-50%);
          transform:translateX(-50%);
  opacity:0;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack2-pop .bp3-panel-stack2-enter, .bp3-panel-stack2-pop .bp3-panel-stack2-appear{
  -webkit-transform:translateX(-50%);
          transform:translateX(-50%);
  opacity:0; }

.bp3-panel-stack2-pop .bp3-panel-stack2-enter-active, .bp3-panel-stack2-pop .bp3-panel-stack2-appear-active{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }

.bp3-panel-stack2-pop .bp3-panel-stack2-exit{
  -webkit-transform:translate(0%);
          transform:translate(0%);
  opacity:1; }

.bp3-panel-stack2-pop .bp3-panel-stack2-exit-active{
  -webkit-transform:translateX(100%);
          transform:translateX(100%);
  opacity:0;
  -webkit-transition-delay:0;
          transition-delay:0;
  -webkit-transition-duration:400ms;
          transition-duration:400ms;
  -webkit-transition-property:opacity, -webkit-transform;
  transition-property:opacity, -webkit-transform;
  transition-property:transform, opacity;
  transition-property:transform, opacity, -webkit-transform;
  -webkit-transition-timing-function:ease;
          transition-timing-function:ease; }
.bp3-popover{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
  -webkit-transform:scale(1);
          transform:scale(1);
  border-radius:3px;
  display:inline-block;
  z-index:20; }
  .bp3-popover .bp3-popover-arrow{
    height:30px;
    position:absolute;
    width:30px; }
    .bp3-popover .bp3-popover-arrow::before{
      height:20px;
      margin:5px;
      width:20px; }
  .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-popover{
    margin-bottom:17px;
    margin-top:-17px; }
    .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-popover > .bp3-popover-arrow{
      bottom:-11px; }
      .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-popover > .bp3-popover-arrow svg{
        -webkit-transform:rotate(-90deg);
                transform:rotate(-90deg); }
  .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-popover{
    margin-left:17px; }
    .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-popover > .bp3-popover-arrow{
      left:-11px; }
      .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-popover > .bp3-popover-arrow svg{
        -webkit-transform:rotate(0);
                transform:rotate(0); }
  .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-popover{
    margin-top:17px; }
    .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-popover > .bp3-popover-arrow{
      top:-11px; }
      .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-popover > .bp3-popover-arrow svg{
        -webkit-transform:rotate(90deg);
                transform:rotate(90deg); }
  .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-popover{
    margin-left:-17px;
    margin-right:17px; }
    .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-popover > .bp3-popover-arrow{
      right:-11px; }
      .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-popover > .bp3-popover-arrow svg{
        -webkit-transform:rotate(180deg);
                transform:rotate(180deg); }
  .bp3-tether-element-attached-middle > .bp3-popover > .bp3-popover-arrow{
    top:50%;
    -webkit-transform:translateY(-50%);
            transform:translateY(-50%); }
  .bp3-tether-element-attached-center > .bp3-popover > .bp3-popover-arrow{
    right:50%;
    -webkit-transform:translateX(50%);
            transform:translateX(50%); }
  .bp3-tether-element-attached-top.bp3-tether-target-attached-top > .bp3-popover > .bp3-popover-arrow{
    top:-0.3934px; }
  .bp3-tether-element-attached-right.bp3-tether-target-attached-right > .bp3-popover > .bp3-popover-arrow{
    right:-0.3934px; }
  .bp3-tether-element-attached-left.bp3-tether-target-attached-left > .bp3-popover > .bp3-popover-arrow{
    left:-0.3934px; }
  .bp3-tether-element-attached-bottom.bp3-tether-target-attached-bottom > .bp3-popover > .bp3-popover-arrow{
    bottom:-0.3934px; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-left > .bp3-popover{
    -webkit-transform-origin:top left;
            transform-origin:top left; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-center > .bp3-popover{
    -webkit-transform-origin:top center;
            transform-origin:top center; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-right > .bp3-popover{
    -webkit-transform-origin:top right;
            transform-origin:top right; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-left > .bp3-popover{
    -webkit-transform-origin:center left;
            transform-origin:center left; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-center > .bp3-popover{
    -webkit-transform-origin:center center;
            transform-origin:center center; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-right > .bp3-popover{
    -webkit-transform-origin:center right;
            transform-origin:center right; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-left > .bp3-popover{
    -webkit-transform-origin:bottom left;
            transform-origin:bottom left; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-center > .bp3-popover{
    -webkit-transform-origin:bottom center;
            transform-origin:bottom center; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-right > .bp3-popover{
    -webkit-transform-origin:bottom right;
            transform-origin:bottom right; }
  .bp3-popover .bp3-popover-content{
    background:#ffffff;
    color:inherit; }
  .bp3-popover .bp3-popover-arrow::before{
    -webkit-box-shadow:1px 1px 6px rgba(16, 22, 26, 0.2);
            box-shadow:1px 1px 6px rgba(16, 22, 26, 0.2); }
  .bp3-popover .bp3-popover-arrow-border{
    fill:#10161a;
    fill-opacity:0.1; }
  .bp3-popover .bp3-popover-arrow-fill{
    fill:#ffffff; }
  .bp3-popover-enter > .bp3-popover, .bp3-popover-appear > .bp3-popover{
    -webkit-transform:scale(0.3);
            transform:scale(0.3); }
  .bp3-popover-enter-active > .bp3-popover, .bp3-popover-appear-active > .bp3-popover{
    -webkit-transform:scale(1);
            transform:scale(1);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }
  .bp3-popover-exit > .bp3-popover{
    -webkit-transform:scale(1);
            transform:scale(1); }
  .bp3-popover-exit-active > .bp3-popover{
    -webkit-transform:scale(0.3);
            transform:scale(0.3);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }
  .bp3-popover .bp3-popover-content{
    border-radius:3px;
    position:relative; }
  .bp3-popover.bp3-popover-content-sizing .bp3-popover-content{
    max-width:350px;
    padding:20px; }
  .bp3-popover-target + .bp3-overlay .bp3-popover.bp3-popover-content-sizing{
    width:350px; }
  .bp3-popover.bp3-minimal{
    margin:0 !important; }
    .bp3-popover.bp3-minimal .bp3-popover-arrow{
      display:none; }
    .bp3-popover.bp3-minimal.bp3-popover{
      -webkit-transform:scale(1);
              transform:scale(1); }
      .bp3-popover-enter > .bp3-popover.bp3-minimal.bp3-popover, .bp3-popover-appear > .bp3-popover.bp3-minimal.bp3-popover{
        -webkit-transform:scale(1);
                transform:scale(1); }
      .bp3-popover-enter-active > .bp3-popover.bp3-minimal.bp3-popover, .bp3-popover-appear-active > .bp3-popover.bp3-minimal.bp3-popover{
        -webkit-transform:scale(1);
                transform:scale(1);
        -webkit-transition-delay:0;
                transition-delay:0;
        -webkit-transition-duration:100ms;
                transition-duration:100ms;
        -webkit-transition-property:-webkit-transform;
        transition-property:-webkit-transform;
        transition-property:transform;
        transition-property:transform, -webkit-transform;
        -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
                transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
      .bp3-popover-exit > .bp3-popover.bp3-minimal.bp3-popover{
        -webkit-transform:scale(1);
                transform:scale(1); }
      .bp3-popover-exit-active > .bp3-popover.bp3-minimal.bp3-popover{
        -webkit-transform:scale(1);
                transform:scale(1);
        -webkit-transition-delay:0;
                transition-delay:0;
        -webkit-transition-duration:100ms;
                transition-duration:100ms;
        -webkit-transition-property:-webkit-transform;
        transition-property:-webkit-transform;
        transition-property:transform;
        transition-property:transform, -webkit-transform;
        -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
                transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-popover.bp3-dark,
  .bp3-dark .bp3-popover{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }
    .bp3-popover.bp3-dark .bp3-popover-content,
    .bp3-dark .bp3-popover .bp3-popover-content{
      background:#30404d;
      color:inherit; }
    .bp3-popover.bp3-dark .bp3-popover-arrow::before,
    .bp3-dark .bp3-popover .bp3-popover-arrow::before{
      -webkit-box-shadow:1px 1px 6px rgba(16, 22, 26, 0.4);
              box-shadow:1px 1px 6px rgba(16, 22, 26, 0.4); }
    .bp3-popover.bp3-dark .bp3-popover-arrow-border,
    .bp3-dark .bp3-popover .bp3-popover-arrow-border{
      fill:#10161a;
      fill-opacity:0.2; }
    .bp3-popover.bp3-dark .bp3-popover-arrow-fill,
    .bp3-dark .bp3-popover .bp3-popover-arrow-fill{
      fill:#30404d; }

.bp3-popover-arrow::before{
  border-radius:2px;
  content:"";
  display:block;
  position:absolute;
  -webkit-transform:rotate(45deg);
          transform:rotate(45deg); }

.bp3-tether-pinned .bp3-popover-arrow{
  display:none; }

.bp3-popover-backdrop{
  background:rgba(255, 255, 255, 0); }

.bp3-transition-container{
  opacity:1;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  z-index:20; }
  .bp3-transition-container.bp3-popover-enter, .bp3-transition-container.bp3-popover-appear{
    opacity:0; }
  .bp3-transition-container.bp3-popover-enter-active, .bp3-transition-container.bp3-popover-appear-active{
    opacity:1;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:100ms;
            transition-duration:100ms;
    -webkit-transition-property:opacity;
    transition-property:opacity;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-transition-container.bp3-popover-exit{
    opacity:1; }
  .bp3-transition-container.bp3-popover-exit-active{
    opacity:0;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:100ms;
            transition-duration:100ms;
    -webkit-transition-property:opacity;
    transition-property:opacity;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-transition-container:focus{
    outline:none; }
  .bp3-transition-container.bp3-popover-leave .bp3-popover-content{
    pointer-events:none; }
  .bp3-transition-container[data-x-out-of-boundaries]{
    display:none; }

span.bp3-popover-target{
  display:inline-block; }

.bp3-popover-wrapper.bp3-fill{
  width:100%; }

.bp3-portal{
  left:0;
  position:absolute;
  right:0;
  top:0; }
@-webkit-keyframes linear-progress-bar-stripes{
  from{
    background-position:0 0; }
  to{
    background-position:30px 0; } }
@keyframes linear-progress-bar-stripes{
  from{
    background-position:0 0; }
  to{
    background-position:30px 0; } }

.bp3-progress-bar{
  background:rgba(92, 112, 128, 0.2);
  border-radius:40px;
  display:block;
  height:8px;
  overflow:hidden;
  position:relative;
  width:100%; }
  .bp3-progress-bar .bp3-progress-meter{
    background:linear-gradient(-45deg, rgba(255, 255, 255, 0.2) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.2) 50%, rgba(255, 255, 255, 0.2) 75%, transparent 75%);
    background-color:rgba(92, 112, 128, 0.8);
    background-size:30px 30px;
    border-radius:40px;
    height:100%;
    position:absolute;
    -webkit-transition:width 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:width 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    width:100%; }
  .bp3-progress-bar:not(.bp3-no-animation):not(.bp3-no-stripes) .bp3-progress-meter{
    animation:linear-progress-bar-stripes 300ms linear infinite reverse; }
  .bp3-progress-bar.bp3-no-stripes .bp3-progress-meter{
    background-image:none; }

.bp3-dark .bp3-progress-bar{
  background:rgba(16, 22, 26, 0.5); }
  .bp3-dark .bp3-progress-bar .bp3-progress-meter{
    background-color:#8a9ba8; }

.bp3-progress-bar.bp3-intent-primary .bp3-progress-meter{
  background-color:#137cbd; }

.bp3-progress-bar.bp3-intent-success .bp3-progress-meter{
  background-color:#0f9960; }

.bp3-progress-bar.bp3-intent-warning .bp3-progress-meter{
  background-color:#d9822b; }

.bp3-progress-bar.bp3-intent-danger .bp3-progress-meter{
  background-color:#db3737; }
@-webkit-keyframes skeleton-glow{
  from{
    background:rgba(206, 217, 224, 0.2);
    border-color:rgba(206, 217, 224, 0.2); }
  to{
    background:rgba(92, 112, 128, 0.2);
    border-color:rgba(92, 112, 128, 0.2); } }
@keyframes skeleton-glow{
  from{
    background:rgba(206, 217, 224, 0.2);
    border-color:rgba(206, 217, 224, 0.2); }
  to{
    background:rgba(92, 112, 128, 0.2);
    border-color:rgba(92, 112, 128, 0.2); } }
.bp3-skeleton{
  -webkit-animation:1000ms linear infinite alternate skeleton-glow;
          animation:1000ms linear infinite alternate skeleton-glow;
  background:rgba(206, 217, 224, 0.2);
  background-clip:padding-box !important;
  border-color:rgba(206, 217, 224, 0.2) !important;
  border-radius:2px;
  -webkit-box-shadow:none !important;
          box-shadow:none !important;
  color:transparent !important;
  cursor:default;
  pointer-events:none;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-skeleton::before, .bp3-skeleton::after,
  .bp3-skeleton *{
    visibility:hidden !important; }
.bp3-slider{
  height:40px;
  min-width:150px;
  width:100%;
  cursor:default;
  outline:none;
  position:relative;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-slider:hover{
    cursor:pointer; }
  .bp3-slider:active{
    cursor:-webkit-grabbing;
    cursor:grabbing; }
  .bp3-slider.bp3-disabled{
    cursor:not-allowed;
    opacity:0.5; }
  .bp3-slider.bp3-slider-unlabeled{
    height:16px; }

.bp3-slider-track,
.bp3-slider-progress{
  height:6px;
  left:0;
  right:0;
  top:5px;
  position:absolute; }

.bp3-slider-track{
  border-radius:3px;
  overflow:hidden; }

.bp3-slider-progress{
  background:rgba(92, 112, 128, 0.2); }
  .bp3-dark .bp3-slider-progress{
    background:rgba(16, 22, 26, 0.5); }
  .bp3-slider-progress.bp3-intent-primary{
    background-color:#137cbd; }
  .bp3-slider-progress.bp3-intent-success{
    background-color:#0f9960; }
  .bp3-slider-progress.bp3-intent-warning{
    background-color:#d9822b; }
  .bp3-slider-progress.bp3-intent-danger{
    background-color:#db3737; }

.bp3-slider-handle{
  background-color:#f5f8fa;
  background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.8)), to(rgba(255, 255, 255, 0)));
  background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0));
  -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
          box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
  color:#182026;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
  cursor:pointer;
  height:16px;
  left:0;
  position:absolute;
  top:0;
  width:16px; }
  .bp3-slider-handle:hover{
    background-clip:padding-box;
    background-color:#ebf1f5;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1); }
  .bp3-slider-handle:active, .bp3-slider-handle.bp3-active{
    background-color:#d8e1e8;
    background-image:none;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
  .bp3-slider-handle:disabled, .bp3-slider-handle.bp3-disabled{
    background-color:rgba(206, 217, 224, 0.5);
    background-image:none;
    -webkit-box-shadow:none;
            box-shadow:none;
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed;
    outline:none; }
    .bp3-slider-handle:disabled.bp3-active, .bp3-slider-handle:disabled.bp3-active:hover, .bp3-slider-handle.bp3-disabled.bp3-active, .bp3-slider-handle.bp3-disabled.bp3-active:hover{
      background:rgba(206, 217, 224, 0.7); }
  .bp3-slider-handle:focus{
    z-index:1; }
  .bp3-slider-handle:hover{
    background-clip:padding-box;
    background-color:#ebf1f5;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 -1px 0 rgba(16, 22, 26, 0.1);
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 1px 1px rgba(16, 22, 26, 0.2);
    cursor:-webkit-grab;
    cursor:grab;
    z-index:2; }
  .bp3-slider-handle.bp3-active{
    background-color:#d8e1e8;
    background-image:none;
    -webkit-box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
            box-shadow:inset 0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 2px rgba(16, 22, 26, 0.2);
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 1px rgba(16, 22, 26, 0.1);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), inset 0 1px 1px rgba(16, 22, 26, 0.1);
    cursor:-webkit-grabbing;
    cursor:grabbing; }
  .bp3-disabled .bp3-slider-handle{
    background:#bfccd6;
    -webkit-box-shadow:none;
            box-shadow:none;
    pointer-events:none; }
  .bp3-dark .bp3-slider-handle{
    background-color:#394b59;
    background-image:-webkit-gradient(linear, left top, left bottom, from(rgba(255, 255, 255, 0.05)), to(rgba(255, 255, 255, 0)));
    background-image:linear-gradient(to bottom, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0));
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
    color:#f5f8fa; }
    .bp3-dark .bp3-slider-handle:hover, .bp3-dark .bp3-slider-handle:active, .bp3-dark .bp3-slider-handle.bp3-active{
      color:#f5f8fa; }
    .bp3-dark .bp3-slider-handle:hover{
      background-color:#30404d;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-slider-handle:active, .bp3-dark .bp3-slider-handle.bp3-active{
      background-color:#202b33;
      background-image:none;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.6), inset 0 1px 2px rgba(16, 22, 26, 0.2); }
    .bp3-dark .bp3-slider-handle:disabled, .bp3-dark .bp3-slider-handle.bp3-disabled{
      background-color:rgba(57, 75, 89, 0.5);
      background-image:none;
      -webkit-box-shadow:none;
              box-shadow:none;
      color:rgba(167, 182, 194, 0.6); }
      .bp3-dark .bp3-slider-handle:disabled.bp3-active, .bp3-dark .bp3-slider-handle.bp3-disabled.bp3-active{
        background:rgba(57, 75, 89, 0.7); }
    .bp3-dark .bp3-slider-handle .bp3-button-spinner .bp3-spinner-head{
      background:rgba(16, 22, 26, 0.5);
      stroke:#8a9ba8; }
    .bp3-dark .bp3-slider-handle, .bp3-dark .bp3-slider-handle:hover{
      background-color:#394b59; }
    .bp3-dark .bp3-slider-handle.bp3-active{
      background-color:#293742; }
  .bp3-dark .bp3-disabled .bp3-slider-handle{
    background:#5c7080;
    border-color:#5c7080;
    -webkit-box-shadow:none;
            box-shadow:none; }
  .bp3-slider-handle .bp3-slider-label{
    background:#394b59;
    border-radius:3px;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
    color:#f5f8fa;
    margin-left:8px; }
    .bp3-dark .bp3-slider-handle .bp3-slider-label{
      background:#e1e8ed;
      -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
      color:#394b59; }
    .bp3-disabled .bp3-slider-handle .bp3-slider-label{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-slider-handle.bp3-start, .bp3-slider-handle.bp3-end{
    width:8px; }
  .bp3-slider-handle.bp3-start{
    border-bottom-right-radius:0;
    border-top-right-radius:0; }
  .bp3-slider-handle.bp3-end{
    border-bottom-left-radius:0;
    border-top-left-radius:0;
    margin-left:8px; }
    .bp3-slider-handle.bp3-end .bp3-slider-label{
      margin-left:0; }

.bp3-slider-label{
  -webkit-transform:translate(-50%, 20px);
          transform:translate(-50%, 20px);
  display:inline-block;
  font-size:12px;
  line-height:1;
  padding:2px 5px;
  position:absolute;
  vertical-align:top; }

.bp3-slider.bp3-vertical{
  height:150px;
  min-width:40px;
  width:40px; }
  .bp3-slider.bp3-vertical .bp3-slider-track,
  .bp3-slider.bp3-vertical .bp3-slider-progress{
    bottom:0;
    height:auto;
    left:5px;
    top:0;
    width:6px; }
  .bp3-slider.bp3-vertical .bp3-slider-progress{
    top:auto; }
  .bp3-slider.bp3-vertical .bp3-slider-label{
    -webkit-transform:translate(20px, 50%);
            transform:translate(20px, 50%); }
  .bp3-slider.bp3-vertical .bp3-slider-handle{
    top:auto; }
    .bp3-slider.bp3-vertical .bp3-slider-handle .bp3-slider-label{
      margin-left:0;
      margin-top:-8px; }
    .bp3-slider.bp3-vertical .bp3-slider-handle.bp3-end, .bp3-slider.bp3-vertical .bp3-slider-handle.bp3-start{
      height:8px;
      margin-left:0;
      width:16px; }
    .bp3-slider.bp3-vertical .bp3-slider-handle.bp3-start{
      border-bottom-right-radius:3px;
      border-top-left-radius:0; }
      .bp3-slider.bp3-vertical .bp3-slider-handle.bp3-start .bp3-slider-label{
        -webkit-transform:translate(20px);
                transform:translate(20px); }
    .bp3-slider.bp3-vertical .bp3-slider-handle.bp3-end{
      border-bottom-left-radius:0;
      border-bottom-right-radius:0;
      border-top-left-radius:3px;
      margin-bottom:8px; }

@-webkit-keyframes pt-spinner-animation{
  from{
    -webkit-transform:rotate(0deg);
            transform:rotate(0deg); }
  to{
    -webkit-transform:rotate(360deg);
            transform:rotate(360deg); } }

@keyframes pt-spinner-animation{
  from{
    -webkit-transform:rotate(0deg);
            transform:rotate(0deg); }
  to{
    -webkit-transform:rotate(360deg);
            transform:rotate(360deg); } }

.bp3-spinner{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-pack:center;
      -ms-flex-pack:center;
          justify-content:center;
  overflow:visible;
  vertical-align:middle; }
  .bp3-spinner svg{
    display:block; }
  .bp3-spinner path{
    fill-opacity:0; }
  .bp3-spinner .bp3-spinner-head{
    stroke:rgba(92, 112, 128, 0.8);
    stroke-linecap:round;
    -webkit-transform-origin:center;
            transform-origin:center;
    -webkit-transition:stroke-dashoffset 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
    transition:stroke-dashoffset 200ms cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-spinner .bp3-spinner-track{
    stroke:rgba(92, 112, 128, 0.2); }

.bp3-spinner-animation{
  -webkit-animation:pt-spinner-animation 500ms linear infinite;
          animation:pt-spinner-animation 500ms linear infinite; }
  .bp3-no-spin > .bp3-spinner-animation{
    -webkit-animation:none;
            animation:none; }

.bp3-dark .bp3-spinner .bp3-spinner-head{
  stroke:#8a9ba8; }

.bp3-dark .bp3-spinner .bp3-spinner-track{
  stroke:rgba(16, 22, 26, 0.5); }

.bp3-spinner.bp3-intent-primary .bp3-spinner-head{
  stroke:#137cbd; }

.bp3-spinner.bp3-intent-success .bp3-spinner-head{
  stroke:#0f9960; }

.bp3-spinner.bp3-intent-warning .bp3-spinner-head{
  stroke:#d9822b; }

.bp3-spinner.bp3-intent-danger .bp3-spinner-head{
  stroke:#db3737; }
.bp3-tabs.bp3-vertical{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex; }
  .bp3-tabs.bp3-vertical > .bp3-tab-list{
    -webkit-box-align:start;
        -ms-flex-align:start;
            align-items:flex-start;
    -webkit-box-orient:vertical;
    -webkit-box-direction:normal;
        -ms-flex-direction:column;
            flex-direction:column; }
    .bp3-tabs.bp3-vertical > .bp3-tab-list .bp3-tab{
      border-radius:3px;
      padding:0 10px;
      width:100%; }
      .bp3-tabs.bp3-vertical > .bp3-tab-list .bp3-tab[aria-selected="true"]{
        background-color:rgba(19, 124, 189, 0.2);
        -webkit-box-shadow:none;
                box-shadow:none; }
    .bp3-tabs.bp3-vertical > .bp3-tab-list .bp3-tab-indicator-wrapper .bp3-tab-indicator{
      background-color:rgba(19, 124, 189, 0.2);
      border-radius:3px;
      bottom:0;
      height:auto;
      left:0;
      right:0;
      top:0; }
  .bp3-tabs.bp3-vertical > .bp3-tab-panel{
    margin-top:0;
    padding-left:20px; }

.bp3-tab-list{
  -webkit-box-align:end;
      -ms-flex-align:end;
          align-items:flex-end;
  border:none;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  list-style:none;
  margin:0;
  padding:0;
  position:relative; }
  .bp3-tab-list > *:not(:last-child){
    margin-right:20px; }

.bp3-tab{
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  word-wrap:normal;
  color:#182026;
  cursor:pointer;
  -webkit-box-flex:0;
      -ms-flex:0 0 auto;
          flex:0 0 auto;
  font-size:14px;
  line-height:30px;
  max-width:100%;
  position:relative;
  vertical-align:top; }
  .bp3-tab a{
    color:inherit;
    display:block;
    text-decoration:none; }
  .bp3-tab-indicator-wrapper ~ .bp3-tab{
    background-color:transparent !important;
    -webkit-box-shadow:none !important;
            box-shadow:none !important; }
  .bp3-tab[aria-disabled="true"]{
    color:rgba(92, 112, 128, 0.6);
    cursor:not-allowed; }
  .bp3-tab[aria-selected="true"]{
    border-radius:0;
    -webkit-box-shadow:inset 0 -3px 0 #106ba3;
            box-shadow:inset 0 -3px 0 #106ba3; }
  .bp3-tab[aria-selected="true"], .bp3-tab:not([aria-disabled="true"]):hover{
    color:#106ba3; }
  .bp3-tab:focus{
    -moz-outline-radius:0; }
  .bp3-large > .bp3-tab{
    font-size:16px;
    line-height:40px; }

.bp3-tab-panel{
  margin-top:20px; }
  .bp3-tab-panel[aria-hidden="true"]{
    display:none; }

.bp3-tab-indicator-wrapper{
  left:0;
  pointer-events:none;
  position:absolute;
  top:0;
  -webkit-transform:translateX(0), translateY(0);
          transform:translateX(0), translateY(0);
  -webkit-transition:height, width, -webkit-transform;
  transition:height, width, -webkit-transform;
  transition:height, transform, width;
  transition:height, transform, width, -webkit-transform;
  -webkit-transition-duration:200ms;
          transition-duration:200ms;
  -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
          transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-tab-indicator-wrapper .bp3-tab-indicator{
    background-color:#106ba3;
    bottom:0;
    height:3px;
    left:0;
    position:absolute;
    right:0; }
  .bp3-tab-indicator-wrapper.bp3-no-animation{
    -webkit-transition:none;
    transition:none; }

.bp3-dark .bp3-tab{
  color:#f5f8fa; }
  .bp3-dark .bp3-tab[aria-disabled="true"]{
    color:rgba(167, 182, 194, 0.6); }
  .bp3-dark .bp3-tab[aria-selected="true"]{
    -webkit-box-shadow:inset 0 -3px 0 #48aff0;
            box-shadow:inset 0 -3px 0 #48aff0; }
  .bp3-dark .bp3-tab[aria-selected="true"], .bp3-dark .bp3-tab:not([aria-disabled="true"]):hover{
    color:#48aff0; }

.bp3-dark .bp3-tab-indicator{
  background-color:#48aff0; }

.bp3-flex-expander{
  -webkit-box-flex:1;
      -ms-flex:1 1;
          flex:1 1; }
.bp3-tag{
  display:-webkit-inline-box;
  display:-ms-inline-flexbox;
  display:inline-flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  background-color:#5c7080;
  border:none;
  border-radius:3px;
  -webkit-box-shadow:none;
          box-shadow:none;
  color:#f5f8fa;
  font-size:12px;
  line-height:16px;
  max-width:100%;
  min-height:20px;
  min-width:20px;
  padding:2px 6px;
  position:relative; }
  .bp3-tag.bp3-interactive{
    cursor:pointer; }
    .bp3-tag.bp3-interactive:hover{
      background-color:rgba(92, 112, 128, 0.85); }
    .bp3-tag.bp3-interactive.bp3-active, .bp3-tag.bp3-interactive:active{
      background-color:rgba(92, 112, 128, 0.7); }
  .bp3-tag > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-tag > .bp3-fill{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-tag::before,
  .bp3-tag > *{
    margin-right:4px; }
  .bp3-tag:empty::before,
  .bp3-tag > :last-child{
    margin-right:0; }
  .bp3-tag:focus{
    outline:rgba(19, 124, 189, 0.6) auto 2px;
    outline-offset:0;
    -moz-outline-radius:6px; }
  .bp3-tag.bp3-round{
    border-radius:30px;
    padding-left:8px;
    padding-right:8px; }
  .bp3-dark .bp3-tag{
    background-color:#bfccd6;
    color:#182026; }
    .bp3-dark .bp3-tag.bp3-interactive{
      cursor:pointer; }
      .bp3-dark .bp3-tag.bp3-interactive:hover{
        background-color:rgba(191, 204, 214, 0.85); }
      .bp3-dark .bp3-tag.bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-interactive:active{
        background-color:rgba(191, 204, 214, 0.7); }
    .bp3-dark .bp3-tag > .bp3-icon, .bp3-dark .bp3-tag .bp3-icon-standard, .bp3-dark .bp3-tag .bp3-icon-large{
      fill:currentColor; }
  .bp3-tag > .bp3-icon, .bp3-tag .bp3-icon-standard, .bp3-tag .bp3-icon-large{
    fill:#ffffff; }
  .bp3-tag.bp3-large,
  .bp3-large .bp3-tag{
    font-size:14px;
    line-height:20px;
    min-height:30px;
    min-width:30px;
    padding:5px 10px; }
    .bp3-tag.bp3-large::before,
    .bp3-tag.bp3-large > *,
    .bp3-large .bp3-tag::before,
    .bp3-large .bp3-tag > *{
      margin-right:7px; }
    .bp3-tag.bp3-large:empty::before,
    .bp3-tag.bp3-large > :last-child,
    .bp3-large .bp3-tag:empty::before,
    .bp3-large .bp3-tag > :last-child{
      margin-right:0; }
    .bp3-tag.bp3-large.bp3-round,
    .bp3-large .bp3-tag.bp3-round{
      padding-left:12px;
      padding-right:12px; }
  .bp3-tag.bp3-intent-primary{
    background:#137cbd;
    color:#ffffff; }
    .bp3-tag.bp3-intent-primary.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-intent-primary.bp3-interactive:hover{
        background-color:rgba(19, 124, 189, 0.85); }
      .bp3-tag.bp3-intent-primary.bp3-interactive.bp3-active, .bp3-tag.bp3-intent-primary.bp3-interactive:active{
        background-color:rgba(19, 124, 189, 0.7); }
  .bp3-tag.bp3-intent-success{
    background:#0f9960;
    color:#ffffff; }
    .bp3-tag.bp3-intent-success.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-intent-success.bp3-interactive:hover{
        background-color:rgba(15, 153, 96, 0.85); }
      .bp3-tag.bp3-intent-success.bp3-interactive.bp3-active, .bp3-tag.bp3-intent-success.bp3-interactive:active{
        background-color:rgba(15, 153, 96, 0.7); }
  .bp3-tag.bp3-intent-warning{
    background:#d9822b;
    color:#ffffff; }
    .bp3-tag.bp3-intent-warning.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-intent-warning.bp3-interactive:hover{
        background-color:rgba(217, 130, 43, 0.85); }
      .bp3-tag.bp3-intent-warning.bp3-interactive.bp3-active, .bp3-tag.bp3-intent-warning.bp3-interactive:active{
        background-color:rgba(217, 130, 43, 0.7); }
  .bp3-tag.bp3-intent-danger{
    background:#db3737;
    color:#ffffff; }
    .bp3-tag.bp3-intent-danger.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-intent-danger.bp3-interactive:hover{
        background-color:rgba(219, 55, 55, 0.85); }
      .bp3-tag.bp3-intent-danger.bp3-interactive.bp3-active, .bp3-tag.bp3-intent-danger.bp3-interactive:active{
        background-color:rgba(219, 55, 55, 0.7); }
  .bp3-tag.bp3-fill{
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    width:100%; }
  .bp3-tag.bp3-minimal > .bp3-icon, .bp3-tag.bp3-minimal .bp3-icon-standard, .bp3-tag.bp3-minimal .bp3-icon-large{
    fill:#5c7080; }
  .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]){
    background-color:rgba(138, 155, 168, 0.2);
    color:#182026; }
    .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive:hover{
        background-color:rgba(92, 112, 128, 0.3); }
      .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive.bp3-active, .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive:active{
        background-color:rgba(92, 112, 128, 0.4); }
    .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]){
      color:#f5f8fa; }
      .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive{
        cursor:pointer; }
        .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive:hover{
          background-color:rgba(191, 204, 214, 0.3); }
        .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]).bp3-interactive:active{
          background-color:rgba(191, 204, 214, 0.4); }
      .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]) > .bp3-icon, .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]) .bp3-icon-standard, .bp3-dark .bp3-tag.bp3-minimal:not([class*="bp3-intent-"]) .bp3-icon-large{
        fill:#a7b6c2; }
  .bp3-tag.bp3-minimal.bp3-intent-primary{
    background-color:rgba(19, 124, 189, 0.15);
    color:#106ba3; }
    .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive:hover{
        background-color:rgba(19, 124, 189, 0.25); }
      .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive.bp3-active, .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive:active{
        background-color:rgba(19, 124, 189, 0.35); }
    .bp3-tag.bp3-minimal.bp3-intent-primary > .bp3-icon, .bp3-tag.bp3-minimal.bp3-intent-primary .bp3-icon-standard, .bp3-tag.bp3-minimal.bp3-intent-primary .bp3-icon-large{
      fill:#137cbd; }
    .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-primary{
      background-color:rgba(19, 124, 189, 0.25);
      color:#48aff0; }
      .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive{
        cursor:pointer; }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive:hover{
          background-color:rgba(19, 124, 189, 0.35); }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-primary.bp3-interactive:active{
          background-color:rgba(19, 124, 189, 0.45); }
  .bp3-tag.bp3-minimal.bp3-intent-success{
    background-color:rgba(15, 153, 96, 0.15);
    color:#0d8050; }
    .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive:hover{
        background-color:rgba(15, 153, 96, 0.25); }
      .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive.bp3-active, .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive:active{
        background-color:rgba(15, 153, 96, 0.35); }
    .bp3-tag.bp3-minimal.bp3-intent-success > .bp3-icon, .bp3-tag.bp3-minimal.bp3-intent-success .bp3-icon-standard, .bp3-tag.bp3-minimal.bp3-intent-success .bp3-icon-large{
      fill:#0f9960; }
    .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-success{
      background-color:rgba(15, 153, 96, 0.25);
      color:#3dcc91; }
      .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive{
        cursor:pointer; }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive:hover{
          background-color:rgba(15, 153, 96, 0.35); }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-success.bp3-interactive:active{
          background-color:rgba(15, 153, 96, 0.45); }
  .bp3-tag.bp3-minimal.bp3-intent-warning{
    background-color:rgba(217, 130, 43, 0.15);
    color:#bf7326; }
    .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive:hover{
        background-color:rgba(217, 130, 43, 0.25); }
      .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive.bp3-active, .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive:active{
        background-color:rgba(217, 130, 43, 0.35); }
    .bp3-tag.bp3-minimal.bp3-intent-warning > .bp3-icon, .bp3-tag.bp3-minimal.bp3-intent-warning .bp3-icon-standard, .bp3-tag.bp3-minimal.bp3-intent-warning .bp3-icon-large{
      fill:#d9822b; }
    .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-warning{
      background-color:rgba(217, 130, 43, 0.25);
      color:#ffb366; }
      .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive{
        cursor:pointer; }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive:hover{
          background-color:rgba(217, 130, 43, 0.35); }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-warning.bp3-interactive:active{
          background-color:rgba(217, 130, 43, 0.45); }
  .bp3-tag.bp3-minimal.bp3-intent-danger{
    background-color:rgba(219, 55, 55, 0.15);
    color:#c23030; }
    .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive{
      cursor:pointer; }
      .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive:hover{
        background-color:rgba(219, 55, 55, 0.25); }
      .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive.bp3-active, .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive:active{
        background-color:rgba(219, 55, 55, 0.35); }
    .bp3-tag.bp3-minimal.bp3-intent-danger > .bp3-icon, .bp3-tag.bp3-minimal.bp3-intent-danger .bp3-icon-standard, .bp3-tag.bp3-minimal.bp3-intent-danger .bp3-icon-large{
      fill:#db3737; }
    .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-danger{
      background-color:rgba(219, 55, 55, 0.25);
      color:#ff7373; }
      .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive{
        cursor:pointer; }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive:hover{
          background-color:rgba(219, 55, 55, 0.35); }
        .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive.bp3-active, .bp3-dark .bp3-tag.bp3-minimal.bp3-intent-danger.bp3-interactive:active{
          background-color:rgba(219, 55, 55, 0.45); }

.bp3-tag-remove{
  background:none;
  border:none;
  color:inherit;
  cursor:pointer;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  margin-bottom:-2px;
  margin-right:-6px !important;
  margin-top:-2px;
  opacity:0.5;
  padding:2px;
  padding-left:0; }
  .bp3-tag-remove:hover{
    background:none;
    opacity:0.8;
    text-decoration:none; }
  .bp3-tag-remove:active{
    opacity:1; }
  .bp3-tag-remove:empty::before{
    font-family:"Icons16", sans-serif;
    font-size:16px;
    font-style:normal;
    font-weight:400;
    line-height:1;
    -moz-osx-font-smoothing:grayscale;
    -webkit-font-smoothing:antialiased;
    content:""; }
  .bp3-large .bp3-tag-remove{
    margin-right:-10px !important;
    padding:0 5px 0 0; }
    .bp3-large .bp3-tag-remove:empty::before{
      font-family:"Icons20", sans-serif;
      font-size:20px;
      font-style:normal;
      font-weight:400;
      line-height:1; }
.bp3-tag-input{
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  -webkit-box-orient:horizontal;
  -webkit-box-direction:normal;
      -ms-flex-direction:row;
          flex-direction:row;
  -webkit-box-align:start;
      -ms-flex-align:start;
          align-items:flex-start;
  cursor:text;
  height:auto;
  line-height:inherit;
  min-height:30px;
  padding-left:5px;
  padding-right:0; }
  .bp3-tag-input > *{
    -webkit-box-flex:0;
        -ms-flex-positive:0;
            flex-grow:0;
    -ms-flex-negative:0;
        flex-shrink:0; }
  .bp3-tag-input > .bp3-tag-input-values{
    -webkit-box-flex:1;
        -ms-flex-positive:1;
            flex-grow:1;
    -ms-flex-negative:1;
        flex-shrink:1; }
  .bp3-tag-input .bp3-tag-input-icon{
    color:#5c7080;
    margin-left:2px;
    margin-right:7px;
    margin-top:7px; }
  .bp3-tag-input .bp3-tag-input-values{
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex;
    -webkit-box-orient:horizontal;
    -webkit-box-direction:normal;
        -ms-flex-direction:row;
            flex-direction:row;
    -webkit-box-align:center;
        -ms-flex-align:center;
            align-items:center;
    -ms-flex-item-align:stretch;
        align-self:stretch;
    -ms-flex-wrap:wrap;
        flex-wrap:wrap;
    margin-right:7px;
    margin-top:5px;
    min-width:0; }
    .bp3-tag-input .bp3-tag-input-values > *{
      -webkit-box-flex:0;
          -ms-flex-positive:0;
              flex-grow:0;
      -ms-flex-negative:0;
          flex-shrink:0; }
    .bp3-tag-input .bp3-tag-input-values > .bp3-fill{
      -webkit-box-flex:1;
          -ms-flex-positive:1;
              flex-grow:1;
      -ms-flex-negative:1;
          flex-shrink:1; }
    .bp3-tag-input .bp3-tag-input-values::before,
    .bp3-tag-input .bp3-tag-input-values > *{
      margin-right:5px; }
    .bp3-tag-input .bp3-tag-input-values:empty::before,
    .bp3-tag-input .bp3-tag-input-values > :last-child{
      margin-right:0; }
    .bp3-tag-input .bp3-tag-input-values:first-child .bp3-input-ghost:first-child{
      padding-left:5px; }
    .bp3-tag-input .bp3-tag-input-values > *{
      margin-bottom:5px; }
  .bp3-tag-input .bp3-tag{
    overflow-wrap:break-word; }
    .bp3-tag-input .bp3-tag.bp3-active{
      outline:rgba(19, 124, 189, 0.6) auto 2px;
      outline-offset:0;
      -moz-outline-radius:6px; }
  .bp3-tag-input .bp3-input-ghost{
    -webkit-box-flex:1;
        -ms-flex:1 1 auto;
            flex:1 1 auto;
    line-height:20px;
    width:80px; }
    .bp3-tag-input .bp3-input-ghost:disabled, .bp3-tag-input .bp3-input-ghost.bp3-disabled{
      cursor:not-allowed; }
  .bp3-tag-input .bp3-button,
  .bp3-tag-input .bp3-spinner{
    margin:3px;
    margin-left:0; }
  .bp3-tag-input .bp3-button{
    min-height:24px;
    min-width:24px;
    padding:0 7px; }
  .bp3-tag-input.bp3-large{
    height:auto;
    min-height:40px; }
    .bp3-tag-input.bp3-large::before,
    .bp3-tag-input.bp3-large > *{
      margin-right:10px; }
    .bp3-tag-input.bp3-large:empty::before,
    .bp3-tag-input.bp3-large > :last-child{
      margin-right:0; }
    .bp3-tag-input.bp3-large .bp3-tag-input-icon{
      margin-left:5px;
      margin-top:10px; }
    .bp3-tag-input.bp3-large .bp3-input-ghost{
      line-height:30px; }
    .bp3-tag-input.bp3-large .bp3-button{
      min-height:30px;
      min-width:30px;
      padding:5px 10px;
      margin:5px;
      margin-left:0; }
    .bp3-tag-input.bp3-large .bp3-spinner{
      margin:8px;
      margin-left:0; }
  .bp3-tag-input.bp3-active{
    background-color:#ffffff;
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-tag-input.bp3-active.bp3-intent-primary{
      -webkit-box-shadow:0 0 0 1px #106ba3, 0 0 0 3px rgba(16, 107, 163, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #106ba3, 0 0 0 3px rgba(16, 107, 163, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-tag-input.bp3-active.bp3-intent-success{
      -webkit-box-shadow:0 0 0 1px #0d8050, 0 0 0 3px rgba(13, 128, 80, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #0d8050, 0 0 0 3px rgba(13, 128, 80, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-tag-input.bp3-active.bp3-intent-warning{
      -webkit-box-shadow:0 0 0 1px #bf7326, 0 0 0 3px rgba(191, 115, 38, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #bf7326, 0 0 0 3px rgba(191, 115, 38, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
    .bp3-tag-input.bp3-active.bp3-intent-danger{
      -webkit-box-shadow:0 0 0 1px #c23030, 0 0 0 3px rgba(194, 48, 48, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2);
              box-shadow:0 0 0 1px #c23030, 0 0 0 3px rgba(194, 48, 48, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.2); }
  .bp3-dark .bp3-tag-input .bp3-tag-input-icon, .bp3-tag-input.bp3-dark .bp3-tag-input-icon{
    color:#a7b6c2; }
  .bp3-dark .bp3-tag-input .bp3-input-ghost, .bp3-tag-input.bp3-dark .bp3-input-ghost{
    color:#f5f8fa; }
    .bp3-dark .bp3-tag-input .bp3-input-ghost::-webkit-input-placeholder, .bp3-tag-input.bp3-dark .bp3-input-ghost::-webkit-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-tag-input .bp3-input-ghost::-moz-placeholder, .bp3-tag-input.bp3-dark .bp3-input-ghost::-moz-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-tag-input .bp3-input-ghost:-ms-input-placeholder, .bp3-tag-input.bp3-dark .bp3-input-ghost:-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-tag-input .bp3-input-ghost::-ms-input-placeholder, .bp3-tag-input.bp3-dark .bp3-input-ghost::-ms-input-placeholder{
      color:rgba(167, 182, 194, 0.6); }
    .bp3-dark .bp3-tag-input .bp3-input-ghost::placeholder, .bp3-tag-input.bp3-dark .bp3-input-ghost::placeholder{
      color:rgba(167, 182, 194, 0.6); }
  .bp3-dark .bp3-tag-input.bp3-active, .bp3-tag-input.bp3-dark.bp3-active{
    background-color:rgba(16, 22, 26, 0.3);
    -webkit-box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px #137cbd, 0 0 0 1px #137cbd, 0 0 0 3px rgba(19, 124, 189, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-tag-input.bp3-active.bp3-intent-primary, .bp3-tag-input.bp3-dark.bp3-active.bp3-intent-primary{
      -webkit-box-shadow:0 0 0 1px #106ba3, 0 0 0 3px rgba(16, 107, 163, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #106ba3, 0 0 0 3px rgba(16, 107, 163, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-tag-input.bp3-active.bp3-intent-success, .bp3-tag-input.bp3-dark.bp3-active.bp3-intent-success{
      -webkit-box-shadow:0 0 0 1px #0d8050, 0 0 0 3px rgba(13, 128, 80, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #0d8050, 0 0 0 3px rgba(13, 128, 80, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-tag-input.bp3-active.bp3-intent-warning, .bp3-tag-input.bp3-dark.bp3-active.bp3-intent-warning{
      -webkit-box-shadow:0 0 0 1px #bf7326, 0 0 0 3px rgba(191, 115, 38, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #bf7326, 0 0 0 3px rgba(191, 115, 38, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }
    .bp3-dark .bp3-tag-input.bp3-active.bp3-intent-danger, .bp3-tag-input.bp3-dark.bp3-active.bp3-intent-danger{
      -webkit-box-shadow:0 0 0 1px #c23030, 0 0 0 3px rgba(194, 48, 48, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4);
              box-shadow:0 0 0 1px #c23030, 0 0 0 3px rgba(194, 48, 48, 0.3), inset 0 0 0 1px rgba(16, 22, 26, 0.3), inset 0 1px 1px rgba(16, 22, 26, 0.4); }

.bp3-input-ghost{
  background:none;
  border:none;
  -webkit-box-shadow:none;
          box-shadow:none;
  padding:0; }
  .bp3-input-ghost::-webkit-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input-ghost::-moz-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input-ghost:-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input-ghost::-ms-input-placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input-ghost::placeholder{
    color:rgba(92, 112, 128, 0.6);
    opacity:1; }
  .bp3-input-ghost:focus{
    outline:none !important; }
.bp3-toast{
  -webkit-box-align:start;
      -ms-flex-align:start;
          align-items:flex-start;
  background-color:#ffffff;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  margin:20px 0 0;
  max-width:500px;
  min-width:300px;
  pointer-events:all;
  position:relative !important; }
  .bp3-toast.bp3-toast-enter, .bp3-toast.bp3-toast-appear{
    -webkit-transform:translateY(-40px);
            transform:translateY(-40px); }
  .bp3-toast.bp3-toast-enter-active, .bp3-toast.bp3-toast-appear-active{
    -webkit-transform:translateY(0);
            transform:translateY(0);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }
  .bp3-toast.bp3-toast-enter ~ .bp3-toast, .bp3-toast.bp3-toast-appear ~ .bp3-toast{
    -webkit-transform:translateY(-40px);
            transform:translateY(-40px); }
  .bp3-toast.bp3-toast-enter-active ~ .bp3-toast, .bp3-toast.bp3-toast-appear-active ~ .bp3-toast{
    -webkit-transform:translateY(0);
            transform:translateY(0);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11);
            transition-timing-function:cubic-bezier(0.54, 1.12, 0.38, 1.11); }
  .bp3-toast.bp3-toast-exit{
    opacity:1;
    -webkit-filter:blur(0);
            filter:blur(0); }
  .bp3-toast.bp3-toast-exit-active{
    opacity:0;
    -webkit-filter:blur(10px);
            filter:blur(10px);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:300ms;
            transition-duration:300ms;
    -webkit-transition-property:opacity, -webkit-filter;
    transition-property:opacity, -webkit-filter;
    transition-property:opacity, filter;
    transition-property:opacity, filter, -webkit-filter;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-toast.bp3-toast-exit ~ .bp3-toast{
    -webkit-transform:translateY(0);
            transform:translateY(0); }
  .bp3-toast.bp3-toast-exit-active ~ .bp3-toast{
    -webkit-transform:translateY(-40px);
            transform:translateY(-40px);
    -webkit-transition-delay:50ms;
            transition-delay:50ms;
    -webkit-transition-duration:100ms;
            transition-duration:100ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-toast .bp3-button-group{
    -webkit-box-flex:0;
        -ms-flex:0 0 auto;
            flex:0 0 auto;
    padding:5px;
    padding-left:0; }
  .bp3-toast > .bp3-icon{
    color:#5c7080;
    margin:12px;
    margin-right:0; }
  .bp3-toast.bp3-dark,
  .bp3-dark .bp3-toast{
    background-color:#394b59;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }
    .bp3-toast.bp3-dark > .bp3-icon,
    .bp3-dark .bp3-toast > .bp3-icon{
      color:#a7b6c2; }
  .bp3-toast[class*="bp3-intent-"] a{
    color:rgba(255, 255, 255, 0.7); }
    .bp3-toast[class*="bp3-intent-"] a:hover{
      color:#ffffff; }
  .bp3-toast[class*="bp3-intent-"] > .bp3-icon{
    color:#ffffff; }
  .bp3-toast[class*="bp3-intent-"] .bp3-button, .bp3-toast[class*="bp3-intent-"] .bp3-button::before,
  .bp3-toast[class*="bp3-intent-"] .bp3-button .bp3-icon, .bp3-toast[class*="bp3-intent-"] .bp3-button:active{
    color:rgba(255, 255, 255, 0.7) !important; }
  .bp3-toast[class*="bp3-intent-"] .bp3-button:focus{
    outline-color:rgba(255, 255, 255, 0.5); }
  .bp3-toast[class*="bp3-intent-"] .bp3-button:hover{
    background-color:rgba(255, 255, 255, 0.15) !important;
    color:#ffffff !important; }
  .bp3-toast[class*="bp3-intent-"] .bp3-button:active{
    background-color:rgba(255, 255, 255, 0.3) !important;
    color:#ffffff !important; }
  .bp3-toast[class*="bp3-intent-"] .bp3-button::after{
    background:rgba(255, 255, 255, 0.3) !important; }
  .bp3-toast.bp3-intent-primary{
    background-color:#137cbd;
    color:#ffffff; }
  .bp3-toast.bp3-intent-success{
    background-color:#0f9960;
    color:#ffffff; }
  .bp3-toast.bp3-intent-warning{
    background-color:#d9822b;
    color:#ffffff; }
  .bp3-toast.bp3-intent-danger{
    background-color:#db3737;
    color:#ffffff; }

.bp3-toast-message{
  -webkit-box-flex:1;
      -ms-flex:1 1 auto;
          flex:1 1 auto;
  padding:11px;
  word-break:break-word; }

.bp3-toast-container{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box !important;
  display:-ms-flexbox !important;
  display:flex !important;
  -webkit-box-orient:vertical;
  -webkit-box-direction:normal;
      -ms-flex-direction:column;
          flex-direction:column;
  left:0;
  overflow:hidden;
  padding:0 20px 20px;
  pointer-events:none;
  right:0;
  z-index:40; }
  .bp3-toast-container.bp3-toast-container-in-portal{
    position:fixed; }
  .bp3-toast-container.bp3-toast-container-inline{
    position:absolute; }
  .bp3-toast-container.bp3-toast-container-top{
    top:0; }
  .bp3-toast-container.bp3-toast-container-bottom{
    bottom:0;
    -webkit-box-orient:vertical;
    -webkit-box-direction:reverse;
        -ms-flex-direction:column-reverse;
            flex-direction:column-reverse;
    top:auto; }
  .bp3-toast-container.bp3-toast-container-left{
    -webkit-box-align:start;
        -ms-flex-align:start;
            align-items:flex-start; }
  .bp3-toast-container.bp3-toast-container-right{
    -webkit-box-align:end;
        -ms-flex-align:end;
            align-items:flex-end; }

.bp3-toast-container-bottom .bp3-toast.bp3-toast-enter:not(.bp3-toast-enter-active),
.bp3-toast-container-bottom .bp3-toast.bp3-toast-enter:not(.bp3-toast-enter-active) ~ .bp3-toast, .bp3-toast-container-bottom .bp3-toast.bp3-toast-appear:not(.bp3-toast-appear-active),
.bp3-toast-container-bottom .bp3-toast.bp3-toast-appear:not(.bp3-toast-appear-active) ~ .bp3-toast,
.bp3-toast-container-bottom .bp3-toast.bp3-toast-exit-active ~ .bp3-toast,
.bp3-toast-container-bottom .bp3-toast.bp3-toast-leave-active ~ .bp3-toast{
  -webkit-transform:translateY(60px);
          transform:translateY(60px); }
.bp3-tooltip{
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 2px 4px rgba(16, 22, 26, 0.2), 0 8px 24px rgba(16, 22, 26, 0.2);
  -webkit-transform:scale(1);
          transform:scale(1); }
  .bp3-tooltip .bp3-popover-arrow{
    height:22px;
    position:absolute;
    width:22px; }
    .bp3-tooltip .bp3-popover-arrow::before{
      height:14px;
      margin:4px;
      width:14px; }
  .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-tooltip{
    margin-bottom:11px;
    margin-top:-11px; }
    .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-tooltip > .bp3-popover-arrow{
      bottom:-8px; }
      .bp3-tether-element-attached-bottom.bp3-tether-target-attached-top > .bp3-tooltip > .bp3-popover-arrow svg{
        -webkit-transform:rotate(-90deg);
                transform:rotate(-90deg); }
  .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-tooltip{
    margin-left:11px; }
    .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-tooltip > .bp3-popover-arrow{
      left:-8px; }
      .bp3-tether-element-attached-left.bp3-tether-target-attached-right > .bp3-tooltip > .bp3-popover-arrow svg{
        -webkit-transform:rotate(0);
                transform:rotate(0); }
  .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-tooltip{
    margin-top:11px; }
    .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-tooltip > .bp3-popover-arrow{
      top:-8px; }
      .bp3-tether-element-attached-top.bp3-tether-target-attached-bottom > .bp3-tooltip > .bp3-popover-arrow svg{
        -webkit-transform:rotate(90deg);
                transform:rotate(90deg); }
  .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-tooltip{
    margin-left:-11px;
    margin-right:11px; }
    .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-tooltip > .bp3-popover-arrow{
      right:-8px; }
      .bp3-tether-element-attached-right.bp3-tether-target-attached-left > .bp3-tooltip > .bp3-popover-arrow svg{
        -webkit-transform:rotate(180deg);
                transform:rotate(180deg); }
  .bp3-tether-element-attached-middle > .bp3-tooltip > .bp3-popover-arrow{
    top:50%;
    -webkit-transform:translateY(-50%);
            transform:translateY(-50%); }
  .bp3-tether-element-attached-center > .bp3-tooltip > .bp3-popover-arrow{
    right:50%;
    -webkit-transform:translateX(50%);
            transform:translateX(50%); }
  .bp3-tether-element-attached-top.bp3-tether-target-attached-top > .bp3-tooltip > .bp3-popover-arrow{
    top:-0.22183px; }
  .bp3-tether-element-attached-right.bp3-tether-target-attached-right > .bp3-tooltip > .bp3-popover-arrow{
    right:-0.22183px; }
  .bp3-tether-element-attached-left.bp3-tether-target-attached-left > .bp3-tooltip > .bp3-popover-arrow{
    left:-0.22183px; }
  .bp3-tether-element-attached-bottom.bp3-tether-target-attached-bottom > .bp3-tooltip > .bp3-popover-arrow{
    bottom:-0.22183px; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-left > .bp3-tooltip{
    -webkit-transform-origin:top left;
            transform-origin:top left; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-center > .bp3-tooltip{
    -webkit-transform-origin:top center;
            transform-origin:top center; }
  .bp3-tether-element-attached-top.bp3-tether-element-attached-right > .bp3-tooltip{
    -webkit-transform-origin:top right;
            transform-origin:top right; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-left > .bp3-tooltip{
    -webkit-transform-origin:center left;
            transform-origin:center left; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-center > .bp3-tooltip{
    -webkit-transform-origin:center center;
            transform-origin:center center; }
  .bp3-tether-element-attached-middle.bp3-tether-element-attached-right > .bp3-tooltip{
    -webkit-transform-origin:center right;
            transform-origin:center right; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-left > .bp3-tooltip{
    -webkit-transform-origin:bottom left;
            transform-origin:bottom left; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-center > .bp3-tooltip{
    -webkit-transform-origin:bottom center;
            transform-origin:bottom center; }
  .bp3-tether-element-attached-bottom.bp3-tether-element-attached-right > .bp3-tooltip{
    -webkit-transform-origin:bottom right;
            transform-origin:bottom right; }
  .bp3-tooltip .bp3-popover-content{
    background:#394b59;
    color:#f5f8fa; }
  .bp3-tooltip .bp3-popover-arrow::before{
    -webkit-box-shadow:1px 1px 6px rgba(16, 22, 26, 0.2);
            box-shadow:1px 1px 6px rgba(16, 22, 26, 0.2); }
  .bp3-tooltip .bp3-popover-arrow-border{
    fill:#10161a;
    fill-opacity:0.1; }
  .bp3-tooltip .bp3-popover-arrow-fill{
    fill:#394b59; }
  .bp3-popover-enter > .bp3-tooltip, .bp3-popover-appear > .bp3-tooltip{
    -webkit-transform:scale(0.8);
            transform:scale(0.8); }
  .bp3-popover-enter-active > .bp3-tooltip, .bp3-popover-appear-active > .bp3-tooltip{
    -webkit-transform:scale(1);
            transform:scale(1);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:100ms;
            transition-duration:100ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-popover-exit > .bp3-tooltip{
    -webkit-transform:scale(1);
            transform:scale(1); }
  .bp3-popover-exit-active > .bp3-tooltip{
    -webkit-transform:scale(0.8);
            transform:scale(0.8);
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:100ms;
            transition-duration:100ms;
    -webkit-transition-property:-webkit-transform;
    transition-property:-webkit-transform;
    transition-property:transform;
    transition-property:transform, -webkit-transform;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-tooltip .bp3-popover-content{
    padding:10px 12px; }
  .bp3-tooltip.bp3-dark,
  .bp3-dark .bp3-tooltip{
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 2px 4px rgba(16, 22, 26, 0.4), 0 8px 24px rgba(16, 22, 26, 0.4); }
    .bp3-tooltip.bp3-dark .bp3-popover-content,
    .bp3-dark .bp3-tooltip .bp3-popover-content{
      background:#e1e8ed;
      color:#394b59; }
    .bp3-tooltip.bp3-dark .bp3-popover-arrow::before,
    .bp3-dark .bp3-tooltip .bp3-popover-arrow::before{
      -webkit-box-shadow:1px 1px 6px rgba(16, 22, 26, 0.4);
              box-shadow:1px 1px 6px rgba(16, 22, 26, 0.4); }
    .bp3-tooltip.bp3-dark .bp3-popover-arrow-border,
    .bp3-dark .bp3-tooltip .bp3-popover-arrow-border{
      fill:#10161a;
      fill-opacity:0.2; }
    .bp3-tooltip.bp3-dark .bp3-popover-arrow-fill,
    .bp3-dark .bp3-tooltip .bp3-popover-arrow-fill{
      fill:#e1e8ed; }
  .bp3-tooltip.bp3-intent-primary .bp3-popover-content{
    background:#137cbd;
    color:#ffffff; }
  .bp3-tooltip.bp3-intent-primary .bp3-popover-arrow-fill{
    fill:#137cbd; }
  .bp3-tooltip.bp3-intent-success .bp3-popover-content{
    background:#0f9960;
    color:#ffffff; }
  .bp3-tooltip.bp3-intent-success .bp3-popover-arrow-fill{
    fill:#0f9960; }
  .bp3-tooltip.bp3-intent-warning .bp3-popover-content{
    background:#d9822b;
    color:#ffffff; }
  .bp3-tooltip.bp3-intent-warning .bp3-popover-arrow-fill{
    fill:#d9822b; }
  .bp3-tooltip.bp3-intent-danger .bp3-popover-content{
    background:#db3737;
    color:#ffffff; }
  .bp3-tooltip.bp3-intent-danger .bp3-popover-arrow-fill{
    fill:#db3737; }

.bp3-tooltip-indicator{
  border-bottom:dotted 1px;
  cursor:help; }
.bp3-tree .bp3-icon, .bp3-tree .bp3-icon-standard, .bp3-tree .bp3-icon-large{
  color:#5c7080; }
  .bp3-tree .bp3-icon.bp3-intent-primary, .bp3-tree .bp3-icon-standard.bp3-intent-primary, .bp3-tree .bp3-icon-large.bp3-intent-primary{
    color:#137cbd; }
  .bp3-tree .bp3-icon.bp3-intent-success, .bp3-tree .bp3-icon-standard.bp3-intent-success, .bp3-tree .bp3-icon-large.bp3-intent-success{
    color:#0f9960; }
  .bp3-tree .bp3-icon.bp3-intent-warning, .bp3-tree .bp3-icon-standard.bp3-intent-warning, .bp3-tree .bp3-icon-large.bp3-intent-warning{
    color:#d9822b; }
  .bp3-tree .bp3-icon.bp3-intent-danger, .bp3-tree .bp3-icon-standard.bp3-intent-danger, .bp3-tree .bp3-icon-large.bp3-intent-danger{
    color:#db3737; }

.bp3-tree-node-list{
  list-style:none;
  margin:0;
  padding-left:0; }

.bp3-tree-root{
  background-color:transparent;
  cursor:default;
  padding-left:0;
  position:relative; }

.bp3-tree-node-content-0{
  padding-left:0px; }

.bp3-tree-node-content-1{
  padding-left:23px; }

.bp3-tree-node-content-2{
  padding-left:46px; }

.bp3-tree-node-content-3{
  padding-left:69px; }

.bp3-tree-node-content-4{
  padding-left:92px; }

.bp3-tree-node-content-5{
  padding-left:115px; }

.bp3-tree-node-content-6{
  padding-left:138px; }

.bp3-tree-node-content-7{
  padding-left:161px; }

.bp3-tree-node-content-8{
  padding-left:184px; }

.bp3-tree-node-content-9{
  padding-left:207px; }

.bp3-tree-node-content-10{
  padding-left:230px; }

.bp3-tree-node-content-11{
  padding-left:253px; }

.bp3-tree-node-content-12{
  padding-left:276px; }

.bp3-tree-node-content-13{
  padding-left:299px; }

.bp3-tree-node-content-14{
  padding-left:322px; }

.bp3-tree-node-content-15{
  padding-left:345px; }

.bp3-tree-node-content-16{
  padding-left:368px; }

.bp3-tree-node-content-17{
  padding-left:391px; }

.bp3-tree-node-content-18{
  padding-left:414px; }

.bp3-tree-node-content-19{
  padding-left:437px; }

.bp3-tree-node-content-20{
  padding-left:460px; }

.bp3-tree-node-content{
  -webkit-box-align:center;
      -ms-flex-align:center;
          align-items:center;
  display:-webkit-box;
  display:-ms-flexbox;
  display:flex;
  height:30px;
  padding-right:5px;
  width:100%; }
  .bp3-tree-node-content:hover{
    background-color:rgba(191, 204, 214, 0.4); }

.bp3-tree-node-caret,
.bp3-tree-node-caret-none{
  min-width:30px; }

.bp3-tree-node-caret{
  color:#5c7080;
  cursor:pointer;
  padding:7px;
  -webkit-transform:rotate(0deg);
          transform:rotate(0deg);
  -webkit-transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:-webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9);
  transition:transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9), -webkit-transform 200ms cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-tree-node-caret:hover{
    color:#182026; }
  .bp3-dark .bp3-tree-node-caret{
    color:#a7b6c2; }
    .bp3-dark .bp3-tree-node-caret:hover{
      color:#f5f8fa; }
  .bp3-tree-node-caret.bp3-tree-node-caret-open{
    -webkit-transform:rotate(90deg);
            transform:rotate(90deg); }
  .bp3-tree-node-caret.bp3-icon-standard::before{
    content:""; }

.bp3-tree-node-icon{
  margin-right:7px;
  position:relative; }

.bp3-tree-node-label{
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  word-wrap:normal;
  -webkit-box-flex:1;
      -ms-flex:1 1 auto;
          flex:1 1 auto;
  position:relative;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-tree-node-label span{
    display:inline; }

.bp3-tree-node-secondary-label{
  padding:0 5px;
  -webkit-user-select:none;
     -moz-user-select:none;
      -ms-user-select:none;
          user-select:none; }
  .bp3-tree-node-secondary-label .bp3-popover-wrapper,
  .bp3-tree-node-secondary-label .bp3-popover-target{
    -webkit-box-align:center;
        -ms-flex-align:center;
            align-items:center;
    display:-webkit-box;
    display:-ms-flexbox;
    display:flex; }

.bp3-tree-node.bp3-disabled .bp3-tree-node-content{
  background-color:inherit;
  color:rgba(92, 112, 128, 0.6);
  cursor:not-allowed; }

.bp3-tree-node.bp3-disabled .bp3-tree-node-caret,
.bp3-tree-node.bp3-disabled .bp3-tree-node-icon{
  color:rgba(92, 112, 128, 0.6);
  cursor:not-allowed; }

.bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content{
  background-color:#137cbd; }
  .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content,
  .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content .bp3-icon, .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content .bp3-icon-standard, .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content .bp3-icon-large{
    color:#ffffff; }
  .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content .bp3-tree-node-caret::before{
    color:rgba(255, 255, 255, 0.7); }
  .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content .bp3-tree-node-caret:hover::before{
    color:#ffffff; }

.bp3-dark .bp3-tree-node-content:hover{
  background-color:rgba(92, 112, 128, 0.3); }

.bp3-dark .bp3-tree .bp3-icon, .bp3-dark .bp3-tree .bp3-icon-standard, .bp3-dark .bp3-tree .bp3-icon-large{
  color:#a7b6c2; }
  .bp3-dark .bp3-tree .bp3-icon.bp3-intent-primary, .bp3-dark .bp3-tree .bp3-icon-standard.bp3-intent-primary, .bp3-dark .bp3-tree .bp3-icon-large.bp3-intent-primary{
    color:#137cbd; }
  .bp3-dark .bp3-tree .bp3-icon.bp3-intent-success, .bp3-dark .bp3-tree .bp3-icon-standard.bp3-intent-success, .bp3-dark .bp3-tree .bp3-icon-large.bp3-intent-success{
    color:#0f9960; }
  .bp3-dark .bp3-tree .bp3-icon.bp3-intent-warning, .bp3-dark .bp3-tree .bp3-icon-standard.bp3-intent-warning, .bp3-dark .bp3-tree .bp3-icon-large.bp3-intent-warning{
    color:#d9822b; }
  .bp3-dark .bp3-tree .bp3-icon.bp3-intent-danger, .bp3-dark .bp3-tree .bp3-icon-standard.bp3-intent-danger, .bp3-dark .bp3-tree .bp3-icon-large.bp3-intent-danger{
    color:#db3737; }

.bp3-dark .bp3-tree-node.bp3-tree-node-selected > .bp3-tree-node-content{
  background-color:#137cbd; }
.bp3-omnibar{
  -webkit-filter:blur(0);
          filter:blur(0);
  opacity:1;
  background-color:#ffffff;
  border-radius:3px;
  -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
          box-shadow:0 0 0 1px rgba(16, 22, 26, 0.1), 0 4px 8px rgba(16, 22, 26, 0.2), 0 18px 46px 6px rgba(16, 22, 26, 0.2);
  left:calc(50% - 250px);
  top:20vh;
  width:500px;
  z-index:21; }
  .bp3-omnibar.bp3-overlay-enter, .bp3-omnibar.bp3-overlay-appear{
    -webkit-filter:blur(20px);
            filter:blur(20px);
    opacity:0.2; }
  .bp3-omnibar.bp3-overlay-enter-active, .bp3-omnibar.bp3-overlay-appear-active{
    -webkit-filter:blur(0);
            filter:blur(0);
    opacity:1;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:200ms;
            transition-duration:200ms;
    -webkit-transition-property:opacity, -webkit-filter;
    transition-property:opacity, -webkit-filter;
    transition-property:filter, opacity;
    transition-property:filter, opacity, -webkit-filter;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-omnibar.bp3-overlay-exit{
    -webkit-filter:blur(0);
            filter:blur(0);
    opacity:1; }
  .bp3-omnibar.bp3-overlay-exit-active{
    -webkit-filter:blur(20px);
            filter:blur(20px);
    opacity:0.2;
    -webkit-transition-delay:0;
            transition-delay:0;
    -webkit-transition-duration:200ms;
            transition-duration:200ms;
    -webkit-transition-property:opacity, -webkit-filter;
    transition-property:opacity, -webkit-filter;
    transition-property:filter, opacity;
    transition-property:filter, opacity, -webkit-filter;
    -webkit-transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9);
            transition-timing-function:cubic-bezier(0.4, 1, 0.75, 0.9); }
  .bp3-omnibar .bp3-input{
    background-color:transparent;
    border-radius:0; }
    .bp3-omnibar .bp3-input, .bp3-omnibar .bp3-input:focus{
      -webkit-box-shadow:none;
              box-shadow:none; }
  .bp3-omnibar .bp3-menu{
    background-color:transparent;
    border-radius:0;
    -webkit-box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.15);
            box-shadow:inset 0 1px 0 rgba(16, 22, 26, 0.15);
    max-height:calc(60vh - 40px);
    overflow:auto; }
    .bp3-omnibar .bp3-menu:empty{
      display:none; }
  .bp3-dark .bp3-omnibar, .bp3-omnibar.bp3-dark{
    background-color:#30404d;
    -webkit-box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4);
            box-shadow:0 0 0 1px rgba(16, 22, 26, 0.2), 0 4px 8px rgba(16, 22, 26, 0.4), 0 18px 46px 6px rgba(16, 22, 26, 0.4); }

.bp3-omnibar-overlay .bp3-overlay-backdrop{
  background-color:rgba(16, 22, 26, 0.2); }

.bp3-select-popover .bp3-popover-content{
  padding:5px; }

.bp3-select-popover .bp3-input-group{
  margin-bottom:0; }

.bp3-select-popover .bp3-menu{
  max-height:300px;
  max-width:400px;
  overflow:auto;
  padding:0; }
  .bp3-select-popover .bp3-menu:not(:first-child){
    padding-top:5px; }

.bp3-multi-select{
  min-width:150px; }

.bp3-multi-select-popover .bp3-menu{
  max-height:300px;
  max-width:400px;
  overflow:auto; }

.bp3-select-popover .bp3-popover-content{
  padding:5px; }

.bp3-select-popover .bp3-input-group{
  margin-bottom:0; }

.bp3-select-popover .bp3-menu{
  max-height:300px;
  max-width:400px;
  overflow:auto;
  padding:0; }
  .bp3-select-popover .bp3-menu:not(:first-child){
    padding-top:5px; }
/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/* This file was auto-generated by ensureUiComponents() in @jupyterlab/buildutils */

/**
 * (DEPRECATED) Support for consuming icons as CSS background images
 */

/* Icons urls */

:root {
  --jp-icon-add: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTE5IDEzaC02djZoLTJ2LTZINXYtMmg2VjVoMnY2aDZ2MnoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-bug: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIj4KICAgIDxwYXRoIGQ9Ik0yMCA4aC0yLjgxYy0uNDUtLjc4LTEuMDctMS40NS0xLjgyLTEuOTZMMTcgNC40MSAxNS41OSAzbC0yLjE3IDIuMTdDMTIuOTYgNS4wNiAxMi40OSA1IDEyIDVjLS40OSAwLS45Ni4wNi0xLjQxLjE3TDguNDEgMyA3IDQuNDFsMS42MiAxLjYzQzcuODggNi41NSA3LjI2IDcuMjIgNi44MSA4SDR2MmgyLjA5Yy0uMDUuMzMtLjA5LjY2LS4wOSAxdjFINHYyaDJ2MWMwIC4zNC4wNC42Ny4wOSAxSDR2MmgyLjgxYzEuMDQgMS43OSAyLjk3IDMgNS4xOSAzczQuMTUtMS4yMSA1LjE5LTNIMjB2LTJoLTIuMDljLjA1LS4zMy4wOS0uNjYuMDktMXYtMWgydi0yaC0ydi0xYzAtLjM0LS4wNC0uNjctLjA5LTFIMjBWOHptLTYgOGgtNHYtMmg0djJ6bTAtNGgtNHYtMmg0djJ6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-build: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTYiIHZpZXdCb3g9IjAgMCAyNCAyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTE0LjkgMTcuNDVDMTYuMjUgMTcuNDUgMTcuMzUgMTYuMzUgMTcuMzUgMTVDMTcuMzUgMTMuNjUgMTYuMjUgMTIuNTUgMTQuOSAxMi41NUMxMy41NCAxMi41NSAxMi40NSAxMy42NSAxMi40NSAxNUMxMi40NSAxNi4zNSAxMy41NCAxNy40NSAxNC45IDE3LjQ1Wk0yMC4xIDE1LjY4TDIxLjU4IDE2Ljg0QzIxLjcxIDE2Ljk1IDIxLjc1IDE3LjEzIDIxLjY2IDE3LjI5TDIwLjI2IDE5LjcxQzIwLjE3IDE5Ljg2IDIwIDE5LjkyIDE5LjgzIDE5Ljg2TDE4LjA5IDE5LjE2QzE3LjczIDE5LjQ0IDE3LjMzIDE5LjY3IDE2LjkxIDE5Ljg1TDE2LjY0IDIxLjdDMTYuNjIgMjEuODcgMTYuNDcgMjIgMTYuMyAyMkgxMy41QzEzLjMyIDIyIDEzLjE4IDIxLjg3IDEzLjE1IDIxLjdMMTIuODkgMTkuODVDMTIuNDYgMTkuNjcgMTIuMDcgMTkuNDQgMTEuNzEgMTkuMTZMOS45NjAwMiAxOS44NkM5LjgxMDAyIDE5LjkyIDkuNjIwMDIgMTkuODYgOS41NDAwMiAxOS43MUw4LjE0MDAyIDE3LjI5QzguMDUwMDIgMTcuMTMgOC4wOTAwMiAxNi45NSA4LjIyMDAyIDE2Ljg0TDkuNzAwMDIgMTUuNjhMOS42NTAwMSAxNUw5LjcwMDAyIDE0LjMxTDguMjIwMDIgMTMuMTZDOC4wOTAwMiAxMy4wNSA4LjA1MDAyIDEyLjg2IDguMTQwMDIgMTIuNzFMOS41NDAwMiAxMC4yOUM5LjYyMDAyIDEwLjEzIDkuODEwMDIgMTAuMDcgOS45NjAwMiAxMC4xM0wxMS43MSAxMC44NEMxMi4wNyAxMC41NiAxMi40NiAxMC4zMiAxMi44OSAxMC4xNUwxMy4xNSA4LjI4OTk4QzEzLjE4IDguMTI5OTggMTMuMzIgNy45OTk5OCAxMy41IDcuOTk5OThIMTYuM0MxNi40NyA3Ljk5OTk4IDE2LjYyIDguMTI5OTggMTYuNjQgOC4yODk5OEwxNi45MSAxMC4xNUMxNy4zMyAxMC4zMiAxNy43MyAxMC41NiAxOC4wOSAxMC44NEwxOS44MyAxMC4xM0MyMCAxMC4wNyAyMC4xNyAxMC4xMyAyMC4yNiAxMC4yOUwyMS42NiAxMi43MUMyMS43NSAxMi44NiAyMS43MSAxMy4wNSAyMS41OCAxMy4xNkwyMC4xIDE0LjMxTDIwLjE1IDE1TDIwLjEgMTUuNjhaIi8+CiAgICA8cGF0aCBkPSJNNy4zMjk2NiA3LjQ0NDU0QzguMDgzMSA3LjAwOTU0IDguMzM5MzIgNi4wNTMzMiA3LjkwNDMyIDUuMjk5ODhDNy40NjkzMiA0LjU0NjQzIDYuNTA4MSA0LjI4MTU2IDUuNzU0NjYgNC43MTY1NkM1LjM5MTc2IDQuOTI2MDggNS4xMjY5NSA1LjI3MTE4IDUuMDE4NDkgNS42NzU5NEM0LjkxMDA0IDYuMDgwNzEgNC45NjY4MiA2LjUxMTk4IDUuMTc2MzQgNi44NzQ4OEM1LjYxMTM0IDcuNjI4MzIgNi41NzYyMiA3Ljg3OTU0IDcuMzI5NjYgNy40NDQ1NFpNOS42NTcxOCA0Ljc5NTkzTDEwLjg2NzIgNC45NTE3OUMxMC45NjI4IDQuOTc3NDEgMTEuMDQwMiA1LjA3MTMzIDExLjAzODIgNS4xODc5M0wxMS4wMzg4IDYuOTg4OTNDMTEuMDQ1NSA3LjEwMDU0IDEwLjk2MTYgNy4xOTUxOCAxMC44NTUgNy4yMTA1NEw5LjY2MDAxIDcuMzgwODNMOS4yMzkxNSA4LjEzMTg4TDkuNjY5NjEgOS4yNTc0NUM5LjcwNzI5IDkuMzYyNzEgOS42NjkzNCA5LjQ3Njk5IDkuNTc0MDggOS41MzE5OUw4LjAxNTIzIDEwLjQzMkM3LjkxMTMxIDEwLjQ5MiA3Ljc5MzM3IDEwLjQ2NzcgNy43MjEwNSAxMC4zODI0TDYuOTg3NDggOS40MzE4OEw2LjEwOTMxIDkuNDMwODNMNS4zNDcwNCAxMC4zOTA1QzUuMjg5MDkgMTAuNDcwMiA1LjE3MzgzIDEwLjQ5MDUgNS4wNzE4NyAxMC40MzM5TDMuNTEyNDUgOS41MzI5M0MzLjQxMDQ5IDkuNDc2MzMgMy4zNzY0NyA5LjM1NzQxIDMuNDEwNzUgOS4yNTY3OUwzLjg2MzQ3IDguMTQwOTNMMy42MTc0OSA3Ljc3NDg4TDMuNDIzNDcgNy4zNzg4M0wyLjIzMDc1IDcuMjEyOTdDMi4xMjY0NyA3LjE5MjM1IDIuMDQwNDkgNy4xMDM0MiAyLjA0MjQ1IDYuOTg2ODJMMi4wNDE4NyA1LjE4NTgyQzIuMDQzODMgNS4wNjkyMiAyLjExOTA5IDQuOTc5NTggMi4yMTcwNCA0Ljk2OTIyTDMuNDIwNjUgNC43OTM5M0wzLjg2NzQ5IDQuMDI3ODhMMy40MTEwNSAyLjkxNzMxQzMuMzczMzcgMi44MTIwNCAzLjQxMTMxIDIuNjk3NzYgMy41MTUyMyAyLjYzNzc2TDUuMDc0MDggMS43Mzc3NkM1LjE2OTM0IDEuNjgyNzYgNS4yODcyOSAxLjcwNzA0IDUuMzU5NjEgMS43OTIzMUw2LjExOTE1IDIuNzI3ODhMNi45ODAwMSAyLjczODkzTDcuNzI0OTYgMS43ODkyMkM3Ljc5MTU2IDEuNzA0NTggNy45MTU0OCAxLjY3OTIyIDguMDA4NzkgMS43NDA4Mkw5LjU2ODIxIDIuNjQxODJDOS42NzAxNyAyLjY5ODQyIDkuNzEyODUgMi44MTIzNCA5LjY4NzIzIDIuOTA3OTdMOS4yMTcxOCA0LjAzMzgzTDkuNDYzMTYgNC4zOTk4OEw5LjY1NzE4IDQuNzk1OTNaIi8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-caret-down-empty-thin: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIwIDIwIj4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSIgc2hhcGUtcmVuZGVyaW5nPSJnZW9tZXRyaWNQcmVjaXNpb24iPgoJCTxwb2x5Z29uIGNsYXNzPSJzdDEiIHBvaW50cz0iOS45LDEzLjYgMy42LDcuNCA0LjQsNi42IDkuOSwxMi4yIDE1LjQsNi43IDE2LjEsNy40ICIvPgoJPC9nPgo8L3N2Zz4K);
  --jp-icon-caret-down-empty: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiIHNoYXBlLXJlbmRlcmluZz0iZ2VvbWV0cmljUHJlY2lzaW9uIj4KICAgIDxwYXRoIGQ9Ik01LjIsNS45TDksOS43bDMuOC0zLjhsMS4yLDEuMmwtNC45LDVsLTQuOS01TDUuMiw1Ljl6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-caret-down: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiIHNoYXBlLXJlbmRlcmluZz0iZ2VvbWV0cmljUHJlY2lzaW9uIj4KICAgIDxwYXRoIGQ9Ik01LjIsNy41TDksMTEuMmwzLjgtMy44SDUuMnoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-caret-left: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSIgc2hhcGUtcmVuZGVyaW5nPSJnZW9tZXRyaWNQcmVjaXNpb24iPgoJCTxwYXRoIGQ9Ik0xMC44LDEyLjhMNy4xLDlsMy44LTMuOGwwLDcuNkgxMC44eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-caret-right: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiIHNoYXBlLXJlbmRlcmluZz0iZ2VvbWV0cmljUHJlY2lzaW9uIj4KICAgIDxwYXRoIGQ9Ik03LjIsNS4yTDEwLjksOWwtMy44LDMuOFY1LjJINy4yeiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-caret-up-empty-thin: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIwIDIwIj4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSIgc2hhcGUtcmVuZGVyaW5nPSJnZW9tZXRyaWNQcmVjaXNpb24iPgoJCTxwb2x5Z29uIGNsYXNzPSJzdDEiIHBvaW50cz0iMTUuNCwxMy4zIDkuOSw3LjcgNC40LDEzLjIgMy42LDEyLjUgOS45LDYuMyAxNi4xLDEyLjYgIi8+Cgk8L2c+Cjwvc3ZnPgo=);
  --jp-icon-caret-up: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSIgc2hhcGUtcmVuZGVyaW5nPSJnZW9tZXRyaWNQcmVjaXNpb24iPgoJCTxwYXRoIGQ9Ik01LjIsMTAuNUw5LDYuOGwzLjgsMy44SDUuMnoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-case-sensitive: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIwIDIwIj4KICA8ZyBjbGFzcz0ianAtaWNvbjIiIGZpbGw9IiM0MTQxNDEiPgogICAgPHJlY3QgeD0iMiIgeT0iMiIgd2lkdGg9IjE2IiBoZWlnaHQ9IjE2Ii8+CiAgPC9nPgogIDxnIGNsYXNzPSJqcC1pY29uLWFjY2VudDIiIGZpbGw9IiNGRkYiPgogICAgPHBhdGggZD0iTTcuNiw4aDAuOWwzLjUsOGgtMS4xTDEwLDE0SDZsLTAuOSwySDRMNy42LDh6IE04LDkuMUw2LjQsMTNoMy4yTDgsOS4xeiIvPgogICAgPHBhdGggZD0iTTE2LjYsOS44Yy0wLjIsMC4xLTAuNCwwLjEtMC43LDAuMWMtMC4yLDAtMC40LTAuMS0wLjYtMC4yYy0wLjEtMC4xLTAuMi0wLjQtMC4yLTAuNyBjLTAuMywwLjMtMC42LDAuNS0wLjksMC43Yy0wLjMsMC4xLTAuNywwLjItMS4xLDAuMmMtMC4zLDAtMC41LDAtMC43LTAuMWMtMC4yLTAuMS0wLjQtMC4yLTAuNi0wLjNjLTAuMi0wLjEtMC4zLTAuMy0wLjQtMC41IGMtMC4xLTAuMi0wLjEtMC40LTAuMS0wLjdjMC0wLjMsMC4xLTAuNiwwLjItMC44YzAuMS0wLjIsMC4zLTAuNCwwLjQtMC41QzEyLDcsMTIuMiw2LjksMTIuNSw2LjhjMC4yLTAuMSwwLjUtMC4xLDAuNy0wLjIgYzAuMy0wLjEsMC41LTAuMSwwLjctMC4xYzAuMiwwLDAuNC0wLjEsMC42LTAuMWMwLjIsMCwwLjMtMC4xLDAuNC0wLjJjMC4xLTAuMSwwLjItMC4yLDAuMi0wLjRjMC0xLTEuMS0xLTEuMy0xIGMtMC40LDAtMS40LDAtMS40LDEuMmgtMC45YzAtMC40LDAuMS0wLjcsMC4yLTFjMC4xLTAuMiwwLjMtMC40LDAuNS0wLjZjMC4yLTAuMiwwLjUtMC4zLDAuOC0wLjNDMTMuMyw0LDEzLjYsNCwxMy45LDQgYzAuMywwLDAuNSwwLDAuOCwwLjFjMC4zLDAsMC41LDAuMSwwLjcsMC4yYzAuMiwwLjEsMC40LDAuMywwLjUsMC41QzE2LDUsMTYsNS4yLDE2LDUuNnYyLjljMCwwLjIsMCwwLjQsMCwwLjUgYzAsMC4xLDAuMSwwLjIsMC4zLDAuMmMwLjEsMCwwLjIsMCwwLjMsMFY5Ljh6IE0xNS4yLDYuOWMtMS4yLDAuNi0zLjEsMC4yLTMuMSwxLjRjMCwxLjQsMy4xLDEsMy4xLTAuNVY2Ljl6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-check: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIj4KICAgIDxwYXRoIGQ9Ik05IDE2LjE3TDQuODMgMTJsLTEuNDIgMS40MUw5IDE5IDIxIDdsLTEuNDEtMS40MXoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-circle-empty: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTEyIDJDNi40NyAyIDIgNi40NyAyIDEyczQuNDcgMTAgMTAgMTAgMTAtNC40NyAxMC0xMFMxNy41MyAyIDEyIDJ6bTAgMThjLTQuNDEgMC04LTMuNTktOC04czMuNTktOCA4LTggOCAzLjU5IDggOC0zLjU5IDgtOCA4eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-circle: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMTggMTgiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPGNpcmNsZSBjeD0iOSIgY3k9IjkiIHI9IjgiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-clear: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8bWFzayBpZD0iZG9udXRIb2xlIj4KICAgIDxyZWN0IHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgZmlsbD0id2hpdGUiIC8+CiAgICA8Y2lyY2xlIGN4PSIxMiIgY3k9IjEyIiByPSI4IiBmaWxsPSJibGFjayIvPgogIDwvbWFzaz4KCiAgPGcgY2xhc3M9ImpwLWljb24zIiBmaWxsPSIjNjE2MTYxIj4KICAgIDxyZWN0IGhlaWdodD0iMTgiIHdpZHRoPSIyIiB4PSIxMSIgeT0iMyIgdHJhbnNmb3JtPSJyb3RhdGUoMzE1LCAxMiwgMTIpIi8+CiAgICA8Y2lyY2xlIGN4PSIxMiIgY3k9IjEyIiByPSIxMCIgbWFzaz0idXJsKCNkb251dEhvbGUpIi8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-close: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbi1ub25lIGpwLWljb24tc2VsZWN0YWJsZS1pbnZlcnNlIGpwLWljb24zLWhvdmVyIiBmaWxsPSJub25lIj4KICAgIDxjaXJjbGUgY3g9IjEyIiBjeT0iMTIiIHI9IjExIi8+CiAgPC9nPgoKICA8ZyBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIGpwLWljb24tYWNjZW50Mi1ob3ZlciIgZmlsbD0iIzYxNjE2MSI+CiAgICA8cGF0aCBkPSJNMTkgNi40MUwxNy41OSA1IDEyIDEwLjU5IDYuNDEgNSA1IDYuNDEgMTAuNTkgMTIgNSAxNy41OSA2LjQxIDE5IDEyIDEzLjQxIDE3LjU5IDE5IDE5IDE3LjU5IDEzLjQxIDEyeiIvPgogIDwvZz4KCiAgPGcgY2xhc3M9ImpwLWljb24tbm9uZSBqcC1pY29uLWJ1c3kiIGZpbGw9Im5vbmUiPgogICAgPGNpcmNsZSBjeD0iMTIiIGN5PSIxMiIgcj0iNyIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-code: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyOCAyOCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CgkJPHBhdGggZD0iTTExLjQgMTguNkw2LjggMTRMMTEuNCA5LjRMMTAgOEw0IDE0TDEwIDIwTDExLjQgMTguNlpNMTYuNiAxOC42TDIxLjIgMTRMMTYuNiA5LjRMMTggOEwyNCAxNEwxOCAyMEwxNi42IDE4LjZWMTguNloiLz4KCTwvZz4KPC9zdmc+Cg==);
  --jp-icon-console: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIwMCAyMDAiPgogIDxnIGNsYXNzPSJqcC1pY29uLWJyYW5kMSBqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiMwMjg4RDEiPgogICAgPHBhdGggZD0iTTIwIDE5LjhoMTYwdjE1OS45SDIweiIvPgogIDwvZz4KICA8ZyBjbGFzcz0ianAtaWNvbi1zZWxlY3RhYmxlLWludmVyc2UiIGZpbGw9IiNmZmYiPgogICAgPHBhdGggZD0iTTEwNSAxMjcuM2g0MHYxMi44aC00MHpNNTEuMSA3N0w3NCA5OS45bC0yMy4zIDIzLjMgMTAuNSAxMC41IDIzLjMtMjMuM0w5NSA5OS45IDg0LjUgODkuNCA2MS42IDY2LjV6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-copy: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMTggMTgiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTExLjksMUgzLjJDMi40LDEsMS43LDEuNywxLjcsMi41djEwLjJoMS41VjIuNWg4LjdWMXogTTE0LjEsMy45aC04Yy0wLjgsMC0xLjUsMC43LTEuNSwxLjV2MTAuMmMwLDAuOCwwLjcsMS41LDEuNSwxLjVoOCBjMC44LDAsMS41LTAuNywxLjUtMS41VjUuNEMxNS41LDQuNiwxNC45LDMuOSwxNC4xLDMuOXogTTE0LjEsMTUuNWgtOFY1LjRoOFYxNS41eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-copyright: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGVuYWJsZS1iYWNrZ3JvdW5kPSJuZXcgMCAwIDI0IDI0IiBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCI+CiAgPGcgY2xhc3M9ImpwLWljb24zIiBmaWxsPSIjNjE2MTYxIj4KICAgIDxwYXRoIGQ9Ik0xMS44OCw5LjE0YzEuMjgsMC4wNiwxLjYxLDEuMTUsMS42MywxLjY2aDEuNzljLTAuMDgtMS45OC0xLjQ5LTMuMTktMy40NS0zLjE5QzkuNjQsNy42MSw4LDksOCwxMi4xNCBjMCwxLjk0LDAuOTMsNC4yNCwzLjg0LDQuMjRjMi4yMiwwLDMuNDEtMS42NSwzLjQ0LTIuOTVoLTEuNzljLTAuMDMsMC41OS0wLjQ1LDEuMzgtMS42MywxLjQ0QzEwLjU1LDE0LjgzLDEwLDEzLjgxLDEwLDEyLjE0IEMxMCw5LjI1LDExLjI4LDkuMTYsMTEuODgsOS4xNHogTTEyLDJDNi40OCwyLDIsNi40OCwyLDEyczQuNDgsMTAsMTAsMTBzMTAtNC40OCwxMC0xMFMxNy41MiwyLDEyLDJ6IE0xMiwyMGMtNC40MSwwLTgtMy41OS04LTggczMuNTktOCw4LThzOCwzLjU5LDgsOFMxNi40MSwyMCwxMiwyMHoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-cut: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTkuNjQgNy42NGMuMjMtLjUuMzYtMS4wNS4zNi0xLjY0IDAtMi4yMS0xLjc5LTQtNC00UzIgMy43OSAyIDZzMS43OSA0IDQgNGMuNTkgMCAxLjE0LS4xMyAxLjY0LS4zNkwxMCAxMmwtMi4zNiAyLjM2QzcuMTQgMTQuMTMgNi41OSAxNCA2IDE0Yy0yLjIxIDAtNCAxLjc5LTQgNHMxLjc5IDQgNCA0IDQtMS43OSA0LTRjMC0uNTktLjEzLTEuMTQtLjM2LTEuNjRMMTIgMTRsNyA3aDN2LTFMOS42NCA3LjY0ek02IDhjLTEuMSAwLTItLjg5LTItMnMuOS0yIDItMiAyIC44OSAyIDItLjkgMi0yIDJ6bTAgMTJjLTEuMSAwLTItLjg5LTItMnMuOS0yIDItMiAyIC44OSAyIDItLjkgMi0yIDJ6bTYtNy41Yy0uMjggMC0uNS0uMjItLjUtLjVzLjIyLS41LjUtLjUuNS4yMi41LjUtLjIyLjUtLjUuNXpNMTkgM2wtNiA2IDIgMiA3LTdWM3oiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-download: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTE5IDloLTRWM0g5djZINWw3IDcgNy03ek01IDE4djJoMTR2LTJINXoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-edit: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTMgMTcuMjVWMjFoMy43NUwxNy44MSA5Ljk0bC0zLjc1LTMuNzVMMyAxNy4yNXpNMjAuNzEgNy4wNGMuMzktLjM5LjM5LTEuMDIgMC0xLjQxbC0yLjM0LTIuMzRjLS4zOS0uMzktMS4wMi0uMzktMS40MSAwbC0xLjgzIDEuODMgMy43NSAzLjc1IDEuODMtMS44M3oiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-ellipses: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPGNpcmNsZSBjeD0iNSIgY3k9IjEyIiByPSIyIi8+CiAgICA8Y2lyY2xlIGN4PSIxMiIgY3k9IjEyIiByPSIyIi8+CiAgICA8Y2lyY2xlIGN4PSIxOSIgY3k9IjEyIiByPSIyIi8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-extension: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTIwLjUgMTFIMTlWN2MwLTEuMS0uOS0yLTItMmgtNFYzLjVDMTMgMi4xMiAxMS44OCAxIDEwLjUgMVM4IDIuMTIgOCAzLjVWNUg0Yy0xLjEgMC0xLjk5LjktMS45OSAydjMuOEgzLjVjMS40OSAwIDIuNyAxLjIxIDIuNyAyLjdzLTEuMjEgMi43LTIuNyAyLjdIMlYyMGMwIDEuMS45IDIgMiAyaDMuOHYtMS41YzAtMS40OSAxLjIxLTIuNyAyLjctMi43IDEuNDkgMCAyLjcgMS4yMSAyLjcgMi43VjIySDE3YzEuMSAwIDItLjkgMi0ydi00aDEuNWMxLjM4IDAgMi41LTEuMTIgMi41LTIuNVMyMS44OCAxMSAyMC41IDExeiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-fast-forward: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTQgMThsOC41LTZMNCA2djEyem05LTEydjEybDguNS02TDEzIDZ6Ii8+CiAgICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-file-upload: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTkgMTZoNnYtNmg0bC03LTctNyA3aDR6bS00IDJoMTR2Mkg1eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-file: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMTkuMyA4LjJsLTUuNS01LjVjLS4zLS4zLS43LS41LTEuMi0uNUgzLjljLS44LjEtMS42LjktMS42IDEuOHYxNC4xYzAgLjkuNyAxLjYgMS42IDEuNmgxNC4yYy45IDAgMS42LS43IDEuNi0xLjZWOS40Yy4xLS41LS4xLS45LS40LTEuMnptLTUuOC0zLjNsMy40IDMuNmgtMy40VjQuOXptMy45IDEyLjdINC43Yy0uMSAwLS4yIDAtLjItLjJWNC43YzAtLjIuMS0uMy4yLS4zaDcuMnY0LjRzMCAuOC4zIDEuMWMuMy4zIDEuMS4zIDEuMS4zaDQuM3Y3LjJzLS4xLjItLjIuMnoiLz4KPC9zdmc+Cg==);
  --jp-icon-filter-list: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTEwIDE4aDR2LTJoLTR2MnpNMyA2djJoMThWNkgzem0zIDdoMTJ2LTJINnYyeiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-folder: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMTAgNEg0Yy0xLjEgMC0xLjk5LjktMS45OSAyTDIgMThjMCAxLjEuOSAyIDIgMmgxNmMxLjEgMCAyLS45IDItMlY4YzAtMS4xLS45LTItMi0yaC04bC0yLTJ6Ii8+Cjwvc3ZnPgo=);
  --jp-icon-html5: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDUxMiA1MTIiPgogIDxwYXRoIGNsYXNzPSJqcC1pY29uMCBqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiMwMDAiIGQ9Ik0xMDguNCAwaDIzdjIyLjhoMjEuMlYwaDIzdjY5aC0yM1Y0NmgtMjF2MjNoLTIzLjJNMjA2IDIzaC0yMC4zVjBoNjMuN3YyM0gyMjl2NDZoLTIzbTUzLjUtNjloMjQuMWwxNC44IDI0LjNMMzEzLjIgMGgyNC4xdjY5aC0yM1YzNC44bC0xNi4xIDI0LjgtMTYuMS0yNC44VjY5aC0yMi42bTg5LjItNjloMjN2NDYuMmgzMi42VjY5aC01NS42Ii8+CiAgPHBhdGggY2xhc3M9ImpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iI2U0NGQyNiIgZD0iTTEwNy42IDQ3MWwtMzMtMzcwLjRoMzYyLjhsLTMzIDM3MC4yTDI1NS43IDUxMiIvPgogIDxwYXRoIGNsYXNzPSJqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiNmMTY1MjkiIGQ9Ik0yNTYgNDgwLjVWMTMxaDE0OC4zTDM3NiA0NDciLz4KICA8cGF0aCBjbGFzcz0ianAtaWNvbi1zZWxlY3RhYmxlLWludmVyc2UiIGZpbGw9IiNlYmViZWIiIGQ9Ik0xNDIgMTc2LjNoMTE0djQ1LjRoLTY0LjJsNC4yIDQ2LjVoNjB2NDUuM0gxNTQuNG0yIDIyLjhIMjAybDMuMiAzNi4zIDUwLjggMTMuNnY0Ny40bC05My4yLTI2Ii8+CiAgPHBhdGggY2xhc3M9ImpwLWljb24tc2VsZWN0YWJsZS1pbnZlcnNlIiBmaWxsPSIjZmZmIiBkPSJNMzY5LjYgMTc2LjNIMjU1Ljh2NDUuNGgxMDkuNm0tNC4xIDQ2LjVIMjU1Ljh2NDUuNGg1NmwtNS4zIDU5LTUwLjcgMTMuNnY0Ny4ybDkzLTI1LjgiLz4KPC9zdmc+Cg==);
  --jp-icon-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8cGF0aCBjbGFzcz0ianAtaWNvbi1icmFuZDQganAtaWNvbi1zZWxlY3RhYmxlLWludmVyc2UiIGZpbGw9IiNGRkYiIGQ9Ik0yLjIgMi4yaDE3LjV2MTcuNUgyLjJ6Ii8+CiAgPHBhdGggY2xhc3M9ImpwLWljb24tYnJhbmQwIGpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iIzNGNTFCNSIgZD0iTTIuMiAyLjJ2MTcuNWgxNy41bC4xLTE3LjVIMi4yem0xMi4xIDIuMmMxLjIgMCAyLjIgMSAyLjIgMi4ycy0xIDIuMi0yLjIgMi4yLTIuMi0xLTIuMi0yLjIgMS0yLjIgMi4yLTIuMnpNNC40IDE3LjZsMy4zLTguOCAzLjMgNi42IDIuMi0zLjIgNC40IDUuNEg0LjR6Ii8+Cjwvc3ZnPgo=);
  --jp-icon-inspector: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMjAgNEg0Yy0xLjEgMC0xLjk5LjktMS45OSAyTDIgMThjMCAxLjEuOSAyIDIgMmgxNmMxLjEgMCAyLS45IDItMlY2YzAtMS4xLS45LTItMi0yem0tNSAxNEg0di00aDExdjR6bTAtNUg0VjloMTF2NHptNSA1aC00VjloNHY5eiIvPgo8L3N2Zz4K);
  --jp-icon-json: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8ZyBjbGFzcz0ianAtaWNvbi13YXJuMSBqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiNGOUE4MjUiPgogICAgPHBhdGggZD0iTTIwLjIgMTEuOGMtMS42IDAtMS43LjUtMS43IDEgMCAuNC4xLjkuMSAxLjMuMS41LjEuOS4xIDEuMyAwIDEuNy0xLjQgMi4zLTMuNSAyLjNoLS45di0xLjloLjVjMS4xIDAgMS40IDAgMS40LS44IDAtLjMgMC0uNi0uMS0xIDAtLjQtLjEtLjgtLjEtMS4yIDAtMS4zIDAtMS44IDEuMy0yLTEuMy0uMi0xLjMtLjctMS4zLTIgMC0uNC4xLS44LjEtMS4yLjEtLjQuMS0uNy4xLTEgMC0uOC0uNC0uNy0xLjQtLjhoLS41VjQuMWguOWMyLjIgMCAzLjUuNyAzLjUgMi4zIDAgLjQtLjEuOS0uMSAxLjMtLjEuNS0uMS45LS4xIDEuMyAwIC41LjIgMSAxLjcgMXYxLjh6TTEuOCAxMC4xYzEuNiAwIDEuNy0uNSAxLjctMSAwLS40LS4xLS45LS4xLTEuMy0uMS0uNS0uMS0uOS0uMS0xLjMgMC0xLjYgMS40LTIuMyAzLjUtMi4zaC45djEuOWgtLjVjLTEgMC0xLjQgMC0xLjQuOCAwIC4zIDAgLjYuMSAxIDAgLjIuMS42LjEgMSAwIDEuMyAwIDEuOC0xLjMgMkM2IDExLjIgNiAxMS43IDYgMTNjMCAuNC0uMS44LS4xIDEuMi0uMS4zLS4xLjctLjEgMSAwIC44LjMuOCAxLjQuOGguNXYxLjloLS45Yy0yLjEgMC0zLjUtLjYtMy41LTIuMyAwLS40LjEtLjkuMS0xLjMuMS0uNS4xLS45LjEtMS4zIDAtLjUtLjItMS0xLjctMXYtMS45eiIvPgogICAgPGNpcmNsZSBjeD0iMTEiIGN5PSIxMy44IiByPSIyLjEiLz4KICAgIDxjaXJjbGUgY3g9IjExIiBjeT0iOC4yIiByPSIyLjEiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-julia: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDMyNSAzMDAiPgogIDxnIGNsYXNzPSJqcC1icmFuZDAganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjY2IzYzMzIj4KICAgIDxwYXRoIGQ9Ik0gMTUwLjg5ODQzOCAyMjUgQyAxNTAuODk4NDM4IDI2Ni40MjE4NzUgMTE3LjMyMDMxMiAzMDAgNzUuODk4NDM4IDMwMCBDIDM0LjQ3NjU2MiAzMDAgMC44OTg0MzggMjY2LjQyMTg3NSAwLjg5ODQzOCAyMjUgQyAwLjg5ODQzOCAxODMuNTc4MTI1IDM0LjQ3NjU2MiAxNTAgNzUuODk4NDM4IDE1MCBDIDExNy4zMjAzMTIgMTUwIDE1MC44OTg0MzggMTgzLjU3ODEyNSAxNTAuODk4NDM4IDIyNSIvPgogIDwvZz4KICA8ZyBjbGFzcz0ianAtYnJhbmQwIGpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iIzM4OTgyNiI+CiAgICA8cGF0aCBkPSJNIDIzNy41IDc1IEMgMjM3LjUgMTE2LjQyMTg3NSAyMDMuOTIxODc1IDE1MCAxNjIuNSAxNTAgQyAxMjEuMDc4MTI1IDE1MCA4Ny41IDExNi40MjE4NzUgODcuNSA3NSBDIDg3LjUgMzMuNTc4MTI1IDEyMS4wNzgxMjUgMCAxNjIuNSAwIEMgMjAzLjkyMTg3NSAwIDIzNy41IDMzLjU3ODEyNSAyMzcuNSA3NSIvPgogIDwvZz4KICA8ZyBjbGFzcz0ianAtYnJhbmQwIGpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iIzk1NThiMiI+CiAgICA8cGF0aCBkPSJNIDMyNC4xMDE1NjIgMjI1IEMgMzI0LjEwMTU2MiAyNjYuNDIxODc1IDI5MC41MjM0MzggMzAwIDI0OS4xMDE1NjIgMzAwIEMgMjA3LjY3OTY4OCAzMDAgMTc0LjEwMTU2MiAyNjYuNDIxODc1IDE3NC4xMDE1NjIgMjI1IEMgMTc0LjEwMTU2MiAxODMuNTc4MTI1IDIwNy42Nzk2ODggMTUwIDI0OS4xMDE1NjIgMTUwIEMgMjkwLjUyMzQzOCAxNTAgMzI0LjEwMTU2MiAxODMuNTc4MTI1IDMyNC4xMDE1NjIgMjI1Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-jupyter-favicon: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTUyIiBoZWlnaHQ9IjE2NSIgdmlld0JveD0iMCAwIDE1MiAxNjUiIHZlcnNpb249IjEuMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbi13YXJuMCIgZmlsbD0iI0YzNzcyNiI+CiAgICA8cGF0aCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgwLjA3ODk0NywgMTEwLjU4MjkyNykiIGQ9Ik03NS45NDIyODQyLDI5LjU4MDQ1NjEgQzQzLjMwMjM5NDcsMjkuNTgwNDU2MSAxNC43OTY3ODMyLDE3LjY1MzQ2MzQgMCwwIEM1LjUxMDgzMjExLDE1Ljg0MDY4MjkgMTUuNzgxNTM4OSwyOS41NjY3NzMyIDI5LjM5MDQ5NDcsMzkuMjc4NDE3MSBDNDIuOTk5Nyw0OC45ODk4NTM3IDU5LjI3MzcsNTQuMjA2NzgwNSA3NS45NjA1Nzg5LDU0LjIwNjc4MDUgQzkyLjY0NzQ1NzksNTQuMjA2NzgwNSAxMDguOTIxNDU4LDQ4Ljk4OTg1MzcgMTIyLjUzMDY2MywzOS4yNzg0MTcxIEMxMzYuMTM5NDUzLDI5LjU2Njc3MzIgMTQ2LjQxMDI4NCwxNS44NDA2ODI5IDE1MS45MjExNTgsMCBDMTM3LjA4Nzg2OCwxNy42NTM0NjM0IDEwOC41ODI1ODksMjkuNTgwNDU2MSA3NS45NDIyODQyLDI5LjU4MDQ1NjEgTDc1Ljk0MjI4NDIsMjkuNTgwNDU2MSBaIiAvPgogICAgPHBhdGggdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMC4wMzczNjgsIDAuNzA0ODc4KSIgZD0iTTc1Ljk3ODQ1NzksMjQuNjI2NDA3MyBDMTA4LjYxODc2MywyNC42MjY0MDczIDEzNy4xMjQ0NTgsMzYuNTUzNDQxNSAxNTEuOTIxMTU4LDU0LjIwNjc4MDUgQzE0Ni40MTAyODQsMzguMzY2MjIyIDEzNi4xMzk0NTMsMjQuNjQwMTMxNyAxMjIuNTMwNjYzLDE0LjkyODQ4NzggQzEwOC45MjE0NTgsNS4yMTY4NDM5IDkyLjY0NzQ1NzksMCA3NS45NjA1Nzg5LDAgQzU5LjI3MzcsMCA0Mi45OTk3LDUuMjE2ODQzOSAyOS4zOTA0OTQ3LDE0LjkyODQ4NzggQzE1Ljc4MTUzODksMjQuNjQwMTMxNyA1LjUxMDgzMjExLDM4LjM2NjIyMiAwLDU0LjIwNjc4MDUgQzE0LjgzMzA4MTYsMzYuNTg5OTI5MyA0My4zMzg1Njg0LDI0LjYyNjQwNzMgNzUuOTc4NDU3OSwyNC42MjY0MDczIEw3NS45Nzg0NTc5LDI0LjYyNjQwNzMgWiIgLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-jupyter: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMzkiIGhlaWdodD0iNTEiIHZpZXdCb3g9IjAgMCAzOSA1MSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMTYzOCAtMjI4MSkiPgogICAgPGcgY2xhc3M9ImpwLWljb24td2FybjAiIGZpbGw9IiNGMzc3MjYiPgogICAgICA8cGF0aCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxNjM5Ljc0IDIzMTEuOTgpIiBkPSJNIDE4LjI2NDYgNy4xMzQxMUMgMTAuNDE0NSA3LjEzNDExIDMuNTU4NzIgNC4yNTc2IDAgMEMgMS4zMjUzOSAzLjgyMDQgMy43OTU1NiA3LjEzMDgxIDcuMDY4NiA5LjQ3MzAzQyAxMC4zNDE3IDExLjgxNTIgMTQuMjU1NyAxMy4wNzM0IDE4LjI2OSAxMy4wNzM0QyAyMi4yODIzIDEzLjA3MzQgMjYuMTk2MyAxMS44MTUyIDI5LjQ2OTQgOS40NzMwM0MgMzIuNzQyNCA3LjEzMDgxIDM1LjIxMjYgMy44MjA0IDM2LjUzOCAwQyAzMi45NzA1IDQuMjU3NiAyNi4xMTQ4IDcuMTM0MTEgMTguMjY0NiA3LjEzNDExWiIvPgogICAgICA8cGF0aCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxNjM5LjczIDIyODUuNDgpIiBkPSJNIDE4LjI3MzMgNS45MzkzMUMgMjYuMTIzNSA1LjkzOTMxIDMyLjk3OTMgOC44MTU4MyAzNi41MzggMTMuMDczNEMgMzUuMjEyNiA5LjI1MzAzIDMyLjc0MjQgNS45NDI2MiAyOS40Njk0IDMuNjAwNEMgMjYuMTk2MyAxLjI1ODE4IDIyLjI4MjMgMCAxOC4yNjkgMEMgMTQuMjU1NyAwIDEwLjM0MTcgMS4yNTgxOCA3LjA2ODYgMy42MDA0QyAzLjc5NTU2IDUuOTQyNjIgMS4zMjUzOSA5LjI1MzAzIDAgMTMuMDczNEMgMy41Njc0NSA4LjgyNDYzIDEwLjQyMzIgNS45MzkzMSAxOC4yNzMzIDUuOTM5MzFaIi8+CiAgICA8L2c+CiAgICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgICA8cGF0aCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxNjY5LjMgMjI4MS4zMSkiIGQ9Ik0gNS44OTM1MyAyLjg0NEMgNS45MTg4OSAzLjQzMTY1IDUuNzcwODUgNC4wMTM2NyA1LjQ2ODE1IDQuNTE2NDVDIDUuMTY1NDUgNS4wMTkyMiA0LjcyMTY4IDUuNDIwMTUgNC4xOTI5OSA1LjY2ODUxQyAzLjY2NDMgNS45MTY4OCAzLjA3NDQ0IDYuMDAxNTEgMi40OTgwNSA1LjkxMTcxQyAxLjkyMTY2IDUuODIxOSAxLjM4NDYzIDUuNTYxNyAwLjk1NDg5OCA1LjE2NDAxQyAwLjUyNTE3IDQuNzY2MzMgMC4yMjIwNTYgNC4yNDkwMyAwLjA4MzkwMzcgMy42Nzc1N0MgLTAuMDU0MjQ4MyAzLjEwNjExIC0wLjAyMTIzIDIuNTA2MTcgMC4xNzg3ODEgMS45NTM2NEMgMC4zNzg3OTMgMS40MDExIDAuNzM2ODA5IDAuOTIwODE3IDEuMjA3NTQgMC41NzM1MzhDIDEuNjc4MjYgMC4yMjYyNTkgMi4yNDA1NSAwLjAyNzU5MTkgMi44MjMyNiAwLjAwMjY3MjI5QyAzLjYwMzg5IC0wLjAzMDcxMTUgNC4zNjU3MyAwLjI0OTc4OSA0Ljk0MTQyIDAuNzgyNTUxQyA1LjUxNzExIDEuMzE1MzEgNS44NTk1NiAyLjA1Njc2IDUuODkzNTMgMi44NDRaIi8+CiAgICAgIDxwYXRoIHRyYW5zZm9ybT0idHJhbnNsYXRlKDE2MzkuOCAyMzIzLjgxKSIgZD0iTSA3LjQyNzg5IDMuNTgzMzhDIDcuNDYwMDggNC4zMjQzIDcuMjczNTUgNS4wNTgxOSA2Ljg5MTkzIDUuNjkyMTNDIDYuNTEwMzEgNi4zMjYwNyA1Ljk1MDc1IDYuODMxNTYgNS4yODQxMSA3LjE0NDZDIDQuNjE3NDcgNy40NTc2MyAzLjg3MzcxIDcuNTY0MTQgMy4xNDcwMiA3LjQ1MDYzQyAyLjQyMDMyIDcuMzM3MTIgMS43NDMzNiA3LjAwODcgMS4yMDE4NCA2LjUwNjk1QyAwLjY2MDMyOCA2LjAwNTIgMC4yNzg2MSA1LjM1MjY4IDAuMTA1MDE3IDQuNjMyMDJDIC0wLjA2ODU3NTcgMy45MTEzNSAtMC4wMjYyMzYxIDMuMTU0OTQgMC4yMjY2NzUgMi40NTg1NkMgMC40Nzk1ODcgMS43NjIxNyAwLjkzMTY5NyAxLjE1NzEzIDEuNTI1NzYgMC43MjAwMzNDIDIuMTE5ODMgMC4yODI5MzUgMi44MjkxNCAwLjAzMzQzOTUgMy41NjM4OSAwLjAwMzEzMzQ0QyA0LjU0NjY3IC0wLjAzNzQwMzMgNS41MDUyOSAwLjMxNjcwNiA2LjIyOTYxIDAuOTg3ODM1QyA2Ljk1MzkzIDEuNjU4OTYgNy4zODQ4NCAyLjU5MjM1IDcuNDI3ODkgMy41ODMzOEwgNy40Mjc4OSAzLjU4MzM4WiIvPgogICAgICA8cGF0aCB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxNjM4LjM2IDIyODYuMDYpIiBkPSJNIDIuMjc0NzEgNC4zOTYyOUMgMS44NDM2MyA0LjQxNTA4IDEuNDE2NzEgNC4zMDQ0NSAxLjA0Nzk5IDQuMDc4NDNDIDAuNjc5MjY4IDMuODUyNCAwLjM4NTMyOCAzLjUyMTE0IDAuMjAzMzcxIDMuMTI2NTZDIDAuMDIxNDEzNiAyLjczMTk4IC0wLjA0MDM3OTggMi4yOTE4MyAwLjAyNTgxMTYgMS44NjE4MUMgMC4wOTIwMDMxIDEuNDMxOCAwLjI4MzIwNCAxLjAzMTI2IDAuNTc1MjEzIDAuNzEwODgzQyAwLjg2NzIyMiAwLjM5MDUxIDEuMjQ2OTEgMC4xNjQ3MDggMS42NjYyMiAwLjA2MjA1OTJDIDIuMDg1NTMgLTAuMDQwNTg5NyAyLjUyNTYxIC0wLjAxNTQ3MTQgMi45MzA3NiAwLjEzNDIzNUMgMy4zMzU5MSAwLjI4Mzk0MSAzLjY4NzkyIDAuNTUxNTA1IDMuOTQyMjIgMC45MDMwNkMgNC4xOTY1MiAxLjI1NDYyIDQuMzQxNjkgMS42NzQzNiA0LjM1OTM1IDIuMTA5MTZDIDQuMzgyOTkgMi42OTEwNyA0LjE3Njc4IDMuMjU4NjkgMy43ODU5NyAzLjY4NzQ2QyAzLjM5NTE2IDQuMTE2MjQgMi44NTE2NiA0LjM3MTE2IDIuMjc0NzEgNC4zOTYyOUwgMi4yNzQ3MSA0LjM5NjI5WiIvPgogICAgPC9nPgogIDwvZz4+Cjwvc3ZnPgo=);
  --jp-icon-jupyterlab-wordmark: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyMDAiIHZpZXdCb3g9IjAgMCAxODYwLjggNDc1Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjIiIGZpbGw9IiM0RTRFNEUiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDQ4MC4xMzY0MDEsIDY0LjI3MTQ5MykiPgogICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMC4wMDAwMDAsIDU4Ljg3NTU2NikiPgogICAgICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgwLjA4NzYwMywgMC4xNDAyOTQpIj4KICAgICAgICA8cGF0aCBkPSJNLTQyNi45LDE2OS44YzAsNDguNy0zLjcsNjQuNy0xMy42LDc2LjRjLTEwLjgsMTAtMjUsMTUuNS0zOS43LDE1LjVsMy43LDI5IGMyMi44LDAuMyw0NC44LTcuOSw2MS45LTIzLjFjMTcuOC0xOC41LDI0LTQ0LjEsMjQtODMuM1YwSC00Mjd2MTcwLjFMLTQyNi45LDE2OS44TC00MjYuOSwxNjkuOHoiLz4KICAgICAgPC9nPgogICAgPC9nPgogICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMTU1LjA0NTI5NiwgNTYuODM3MTA0KSI+CiAgICAgIDxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKDEuNTYyNDUzLCAxLjc5OTg0MikiPgogICAgICAgIDxwYXRoIGQ9Ik0tMzEyLDE0OGMwLDIxLDAsMzkuNSwxLjcsNTUuNGgtMzEuOGwtMi4xLTMzLjNoLTAuOGMtNi43LDExLjYtMTYuNCwyMS4zLTI4LDI3LjkgYy0xMS42LDYuNi0yNC44LDEwLTM4LjIsOS44Yy0zMS40LDAtNjktMTcuNy02OS04OVYwaDM2LjR2MTEyLjdjMCwzOC43LDExLjYsNjQuNyw0NC42LDY0LjdjMTAuMy0wLjIsMjAuNC0zLjUsMjguOS05LjQgYzguNS01LjksMTUuMS0xNC4zLDE4LjktMjMuOWMyLjItNi4xLDMuMy0xMi41LDMuMy0xOC45VjAuMmgzNi40VjE0OEgtMzEyTC0zMTIsMTQ4eiIvPgogICAgICA8L2c+CiAgICA8L2c+CiAgICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgzOTAuMDEzMzIyLCA1My40Nzk2MzgpIj4KICAgICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMS43MDY0NTgsIDAuMjMxNDI1KSI+CiAgICAgICAgPHBhdGggZD0iTS00NzguNiw3MS40YzAtMjYtMC44LTQ3LTEuNy02Ni43aDMyLjdsMS43LDM0LjhoMC44YzcuMS0xMi41LDE3LjUtMjIuOCwzMC4xLTI5LjcgYzEyLjUtNywyNi43LTEwLjMsNDEtOS44YzQ4LjMsMCw4NC43LDQxLjcsODQuNywxMDMuM2MwLDczLjEtNDMuNywxMDkuMi05MSwxMDkuMmMtMTIuMSwwLjUtMjQuMi0yLjItMzUtNy44IGMtMTAuOC01LjYtMTkuOS0xMy45LTI2LjYtMjQuMmgtMC44VjI5MWgtMzZ2LTIyMEwtNDc4LjYsNzEuNEwtNDc4LjYsNzEuNHogTS00NDIuNiwxMjUuNmMwLjEsNS4xLDAuNiwxMC4xLDEuNywxNS4xIGMzLDEyLjMsOS45LDIzLjMsMTkuOCwzMS4xYzkuOSw3LjgsMjIuMSwxMi4xLDM0LjcsMTIuMWMzOC41LDAsNjAuNy0zMS45LDYwLjctNzguNWMwLTQwLjctMjEuMS03NS42LTU5LjUtNzUuNiBjLTEyLjksMC40LTI1LjMsNS4xLTM1LjMsMTMuNGMtOS45LDguMy0xNi45LDE5LjctMTkuNiwzMi40Yy0xLjUsNC45LTIuMywxMC0yLjUsMTUuMVYxMjUuNkwtNDQyLjYsMTI1LjZMLTQ0Mi42LDEyNS42eiIvPgogICAgICA8L2c+CiAgICA8L2c+CiAgICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSg2MDYuNzQwNzI2LCA1Ni44MzcxMDQpIj4KICAgICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMC43NTEyMjYsIDEuOTg5Mjk5KSI+CiAgICAgICAgPHBhdGggZD0iTS00NDAuOCwwbDQzLjcsMTIwLjFjNC41LDEzLjQsOS41LDI5LjQsMTIuOCw0MS43aDAuOGMzLjctMTIuMiw3LjktMjcuNywxMi44LTQyLjQgbDM5LjctMTE5LjJoMzguNUwtMzQ2LjksMTQ1Yy0yNiw2OS43LTQzLjcsMTA1LjQtNjguNiwxMjcuMmMtMTIuNSwxMS43LTI3LjksMjAtNDQuNiwyMy45bC05LjEtMzEuMSBjMTEuNy0zLjksMjIuNS0xMC4xLDMxLjgtMTguMWMxMy4yLTExLjEsMjMuNy0yNS4yLDMwLjYtNDEuMmMxLjUtMi44LDIuNS01LjcsMi45LTguOGMtMC4zLTMuMy0xLjItNi42LTIuNS05LjdMLTQ4MC4yLDAuMSBoMzkuN0wtNDQwLjgsMEwtNDQwLjgsMHoiLz4KICAgICAgPC9nPgogICAgPC9nPgogICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoODIyLjc0ODEwNCwgMC4wMDAwMDApIj4KICAgICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMS40NjQwNTAsIDAuMzc4OTE0KSI+CiAgICAgICAgPHBhdGggZD0iTS00MTMuNywwdjU4LjNoNTJ2MjguMmgtNTJWMTk2YzAsMjUsNywzOS41LDI3LjMsMzkuNWM3LjEsMC4xLDE0LjItMC43LDIxLjEtMi41IGwxLjcsMjcuN2MtMTAuMywzLjctMjEuMyw1LjQtMzIuMiw1Yy03LjMsMC40LTE0LjYtMC43LTIxLjMtMy40Yy02LjgtMi43LTEyLjktNi44LTE3LjktMTIuMWMtMTAuMy0xMC45LTE0LjEtMjktMTQuMS01Mi45IFY4Ni41aC0zMVY1OC4zaDMxVjkuNkwtNDEzLjcsMEwtNDEzLjcsMHoiLz4KICAgICAgPC9nPgogICAgPC9nPgogICAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoOTc0LjQzMzI4NiwgNTMuNDc5NjM4KSI+CiAgICAgIDxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKDAuOTkwMDM0LCAwLjYxMDMzOSkiPgogICAgICAgIDxwYXRoIGQ9Ik0tNDQ1LjgsMTEzYzAuOCw1MCwzMi4yLDcwLjYsNjguNiw3MC42YzE5LDAuNiwzNy45LTMsNTUuMy0xMC41bDYuMiwyNi40IGMtMjAuOSw4LjktNDMuNSwxMy4xLTY2LjIsMTIuNmMtNjEuNSwwLTk4LjMtNDEuMi05OC4zLTEwMi41Qy00ODAuMiw0OC4yLTQ0NC43LDAtMzg2LjUsMGM2NS4yLDAsODIuNyw1OC4zLDgyLjcsOTUuNyBjLTAuMSw1LjgtMC41LDExLjUtMS4yLDE3LjJoLTE0MC42SC00NDUuOEwtNDQ1LjgsMTEzeiBNLTMzOS4yLDg2LjZjMC40LTIzLjUtOS41LTYwLjEtNTAuNC02MC4xIGMtMzYuOCwwLTUyLjgsMzQuNC01NS43LDYwLjFILTMzOS4yTC0zMzkuMiw4Ni42TC0zMzkuMiw4Ni42eiIvPgogICAgICA8L2c+CiAgICA8L2c+CiAgICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxMjAxLjk2MTA1OCwgNTMuNDc5NjM4KSI+CiAgICAgIDxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKDEuMTc5NjQwLCAwLjcwNTA2OCkiPgogICAgICAgIDxwYXRoIGQ9Ik0tNDc4LjYsNjhjMC0yMy45LTAuNC00NC41LTEuNy02My40aDMxLjhsMS4yLDM5LjloMS43YzkuMS0yNy4zLDMxLTQ0LjUsNTUuMy00NC41IGMzLjUtMC4xLDcsMC40LDEwLjMsMS4ydjM0LjhjLTQuMS0wLjktOC4yLTEuMy0xMi40LTEuMmMtMjUuNiwwLTQzLjcsMTkuNy00OC43LDQ3LjRjLTEsNS43LTEuNiwxMS41LTEuNywxNy4ydjEwOC4zaC0zNlY2OCBMLTQ3OC42LDY4eiIvPgogICAgICA8L2c+CiAgICA8L2c+CiAgPC9nPgoKICA8ZyBjbGFzcz0ianAtaWNvbi13YXJuMCIgZmlsbD0iI0YzNzcyNiI+CiAgICA8cGF0aCBkPSJNMTM1Mi4zLDMyNi4yaDM3VjI4aC0zN1YzMjYuMnogTTE2MDQuOCwzMjYuMmMtMi41LTEzLjktMy40LTMxLjEtMy40LTQ4Ljd2LTc2IGMwLTQwLjctMTUuMS04My4xLTc3LjMtODMuMWMtMjUuNiwwLTUwLDcuMS02Ni44LDE4LjFsOC40LDI0LjRjMTQuMy05LjIsMzQtMTUuMSw1My0xNS4xYzQxLjYsMCw0Ni4yLDMwLjIsNDYuMiw0N3Y0LjIgYy03OC42LTAuNC0xMjIuMywyNi41LTEyMi4zLDc1LjZjMCwyOS40LDIxLDU4LjQsNjIuMiw1OC40YzI5LDAsNTAuOS0xNC4zLDYyLjItMzAuMmgxLjNsMi45LDI1LjZIMTYwNC44eiBNMTU2NS43LDI1Ny43IGMwLDMuOC0wLjgsOC0yLjEsMTEuOGMtNS45LDE3LjItMjIuNywzNC00OS4yLDM0Yy0xOC45LDAtMzQuOS0xMS4zLTM0LjktMzUuM2MwLTM5LjUsNDUuOC00Ni42LDg2LjItNDUuOFYyNTcuN3ogTTE2OTguNSwzMjYuMiBsMS43LTMzLjZoMS4zYzE1LjEsMjYuOSwzOC43LDM4LjIsNjguMSwzOC4yYzQ1LjQsMCw5MS4yLTM2LjEsOTEuMi0xMDguOGMwLjQtNjEuNy0zNS4zLTEwMy43LTg1LjctMTAzLjcgYy0zMi44LDAtNTYuMywxNC43LTY5LjMsMzcuNGgtMC44VjI4aC0zNi42djI0NS43YzAsMTguMS0wLjgsMzguNi0xLjcsNTIuNUgxNjk4LjV6IE0xNzA0LjgsMjA4LjJjMC01LjksMS4zLTEwLjksMi4xLTE1LjEgYzcuNi0yOC4xLDMxLjEtNDUuNCw1Ni4zLTQ1LjRjMzkuNSwwLDYwLjUsMzQuOSw2MC41LDc1LjZjMCw0Ni42LTIzLjEsNzguMS02MS44LDc4LjFjLTI2LjksMC00OC4zLTE3LjYtNTUuNS00My4zIGMtMC44LTQuMi0xLjctOC44LTEuNy0xMy40VjIwOC4yeiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-kernel: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uMiIgZmlsbD0iIzYxNjE2MSIgZD0iTTE1IDlIOXY2aDZWOXptLTIgNGgtMnYtMmgydjJ6bTgtMlY5aC0yVjdjMC0xLjEtLjktMi0yLTJoLTJWM2gtMnYyaC0yVjNIOXYySDdjLTEuMSAwLTIgLjktMiAydjJIM3YyaDJ2MkgzdjJoMnYyYzAgMS4xLjkgMiAyIDJoMnYyaDJ2LTJoMnYyaDJ2LTJoMmMxLjEgMCAyLS45IDItMnYtMmgydi0yaC0ydi0yaDJ6bS00IDZIN1Y3aDEwdjEweiIvPgo8L3N2Zz4K);
  --jp-icon-keyboard: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMjAgNUg0Yy0xLjEgMC0xLjk5LjktMS45OSAyTDIgMTdjMCAxLjEuOSAyIDIgMmgxNmMxLjEgMCAyLS45IDItMlY3YzAtMS4xLS45LTItMi0yem0tOSAzaDJ2MmgtMlY4em0wIDNoMnYyaC0ydi0yek04IDhoMnYySDhWOHptMCAzaDJ2Mkg4di0yem0tMSAySDV2LTJoMnYyem0wLTNINVY4aDJ2MnptOSA3SDh2LTJoOHYyem0wLTRoLTJ2LTJoMnYyem0wLTNoLTJWOGgydjJ6bTMgM2gtMnYtMmgydjJ6bTAtM2gtMlY4aDJ2MnoiLz4KPC9zdmc+Cg==);
  --jp-icon-launcher: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMTkgMTlINVY1aDdWM0g1YTIgMiAwIDAwLTIgMnYxNGEyIDIgMCAwMDIgMmgxNGMxLjEgMCAyLS45IDItMnYtN2gtMnY3ek0xNCAzdjJoMy41OWwtOS44MyA5LjgzIDEuNDEgMS40MUwxOSA2LjQxVjEwaDJWM2gtN3oiLz4KPC9zdmc+Cg==);
  --jp-icon-line-form: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICAgIDxwYXRoIGZpbGw9IndoaXRlIiBkPSJNNS44OCA0LjEyTDEzLjc2IDEybC03Ljg4IDcuODhMOCAyMmwxMC0xMEw4IDJ6Ii8+Cjwvc3ZnPgo=);
  --jp-icon-link: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTMuOSAxMmMwLTEuNzEgMS4zOS0zLjEgMy4xLTMuMWg0VjdIN2MtMi43NiAwLTUgMi4yNC01IDVzMi4yNCA1IDUgNWg0di0xLjlIN2MtMS43MSAwLTMuMS0xLjM5LTMuMS0zLjF6TTggMTNoOHYtMkg4djJ6bTktNmgtNHYxLjloNGMxLjcxIDAgMy4xIDEuMzkgMy4xIDMuMXMtMS4zOSAzLjEtMy4xIDMuMWgtNFYxN2g0YzIuNzYgMCA1LTIuMjQgNS01cy0yLjI0LTUtNS01eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-list: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uMiBqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiM2MTYxNjEiIGQ9Ik0xOSA1djE0SDVWNWgxNG0xLjEtMkgzLjljLS41IDAtLjkuNC0uOS45djE2LjJjMCAuNC40LjkuOS45aDE2LjJjLjQgMCAuOS0uNS45LS45VjMuOWMwLS41LS41LS45LS45LS45ek0xMSA3aDZ2MmgtNlY3em0wIDRoNnYyaC02di0yem0wIDRoNnYyaC02ek03IDdoMnYySDd6bTAgNGgydjJIN3ptMCA0aDJ2Mkg3eiIvPgo8L3N2Zz4=);
  --jp-icon-listings-info: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA1MC45NzggNTAuOTc4IiBzdHlsZT0iZW5hYmxlLWJhY2tncm91bmQ6bmV3IDAgMCA1MC45NzggNTAuOTc4OyIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI+Cgk8Zz4KCQk8cGF0aCBzdHlsZT0iZmlsbDojMDEwMDAyOyIgZD0iTTQzLjUyLDcuNDU4QzM4LjcxMSwyLjY0OCwzMi4zMDcsMCwyNS40ODksMEMxOC42NywwLDEyLjI2NiwyLjY0OCw3LjQ1OCw3LjQ1OAoJCQljLTkuOTQzLDkuOTQxLTkuOTQzLDI2LjExOSwwLDM2LjA2MmM0LjgwOSw0LjgwOSwxMS4yMTIsNy40NTYsMTguMDMxLDcuNDU4YzAsMCwwLjAwMSwwLDAuMDAyLDAKCQkJYzYuODE2LDAsMTMuMjIxLTIuNjQ4LDE4LjAyOS03LjQ1OGM0LjgwOS00LjgwOSw3LjQ1Ny0xMS4yMTIsNy40NTctMTguMDNDNTAuOTc3LDE4LjY3LDQ4LjMyOCwxMi4yNjYsNDMuNTIsNy40NTh6CgkJCSBNNDIuMTA2LDQyLjEwNWMtNC40MzIsNC40MzEtMTAuMzMyLDYuODcyLTE2LjYxNSw2Ljg3MmgtMC4wMDJjLTYuMjg1LTAuMDAxLTEyLjE4Ny0yLjQ0MS0xNi42MTctNi44NzIKCQkJYy05LjE2Mi05LjE2My05LjE2Mi0yNC4wNzEsMC0zMy4yMzNDMTMuMzAzLDQuNDQsMTkuMjA0LDIsMjUuNDg5LDJjNi4yODQsMCwxMi4xODYsMi40NCwxNi42MTcsNi44NzIKCQkJYzQuNDMxLDQuNDMxLDYuODcxLDEwLjMzMiw2Ljg3MSwxNi42MTdDNDguOTc3LDMxLjc3Miw0Ni41MzYsMzcuNjc1LDQyLjEwNiw0Mi4xMDV6Ii8+CgkJPHBhdGggc3R5bGU9ImZpbGw6IzAxMDAwMjsiIGQ9Ik0yMy41NzgsMzIuMjE4Yy0wLjAyMy0xLjczNCwwLjE0My0zLjA1OSwwLjQ5Ni0zLjk3MmMwLjM1My0wLjkxMywxLjExLTEuOTk3LDIuMjcyLTMuMjUzCgkJCWMwLjQ2OC0wLjUzNiwwLjkyMy0xLjA2MiwxLjM2Ny0xLjU3NWMwLjYyNi0wLjc1MywxLjEwNC0xLjQ3OCwxLjQzNi0yLjE3NWMwLjMzMS0wLjcwNywwLjQ5NS0xLjU0MSwwLjQ5NS0yLjUKCQkJYzAtMS4wOTYtMC4yNi0yLjA4OC0wLjc3OS0yLjk3OWMtMC41NjUtMC44NzktMS41MDEtMS4zMzYtMi44MDYtMS4zNjljLTEuODAyLDAuMDU3LTIuOTg1LDAuNjY3LTMuNTUsMS44MzIKCQkJYy0wLjMwMSwwLjUzNS0wLjUwMywxLjE0MS0wLjYwNywxLjgxNGMtMC4xMzksMC43MDctMC4yMDcsMS40MzItMC4yMDcsMi4xNzRoLTIuOTM3Yy0wLjA5MS0yLjIwOCwwLjQwNy00LjExNCwxLjQ5My01LjcxOQoJCQljMS4wNjItMS42NCwyLjg1NS0yLjQ4MSw1LjM3OC0yLjUyN2MyLjE2LDAuMDIzLDMuODc0LDAuNjA4LDUuMTQxLDEuNzU4YzEuMjc4LDEuMTYsMS45MjksMi43NjQsMS45NSw0LjgxMQoJCQljMCwxLjE0Mi0wLjEzNywyLjExMS0wLjQxLDIuOTExYy0wLjMwOSwwLjg0NS0wLjczMSwxLjU5My0xLjI2OCwyLjI0M2MtMC40OTIsMC42NS0xLjA2OCwxLjMxOC0xLjczLDIuMDAyCgkJCWMtMC42NSwwLjY5Ny0xLjMxMywxLjQ3OS0xLjk4NywyLjM0NmMtMC4yMzksMC4zNzctMC40MjksMC43NzctMC41NjUsMS4xOTljLTAuMTYsMC45NTktMC4yMTcsMS45NTEtMC4xNzEsMi45NzkKCQkJQzI2LjU4OSwzMi4yMTgsMjMuNTc4LDMyLjIxOCwyMy41NzgsMzIuMjE4eiBNMjMuNTc4LDM4LjIydi0zLjQ4NGgzLjA3NnYzLjQ4NEgyMy41Nzh6Ii8+Cgk8L2c+Cjwvc3ZnPgo=);
  --jp-icon-markdown: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8cGF0aCBjbGFzcz0ianAtaWNvbi1jb250cmFzdDAganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjN0IxRkEyIiBkPSJNNSAxNC45aDEybC02LjEgNnptOS40LTYuOGMwLTEuMy0uMS0yLjktLjEtNC41LS40IDEuNC0uOSAyLjktMS4zIDQuM2wtMS4zIDQuM2gtMkw4LjUgNy45Yy0uNC0xLjMtLjctMi45LTEtNC4zLS4xIDEuNi0uMSAzLjItLjIgNC42TDcgMTIuNEg0LjhsLjctMTFoMy4zTDEwIDVjLjQgMS4yLjcgMi43IDEgMy45LjMtMS4yLjctMi42IDEtMy45bDEuMi0zLjdoMy4zbC42IDExaC0yLjRsLS4zLTQuMnoiLz4KPC9zdmc+Cg==);
  --jp-icon-new-folder: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTIwIDZoLThsLTItMkg0Yy0xLjExIDAtMS45OS44OS0xLjk5IDJMMiAxOGMwIDEuMTEuODkgMiAyIDJoMTZjMS4xMSAwIDItLjg5IDItMlY4YzAtMS4xMS0uODktMi0yLTJ6bS0xIDhoLTN2M2gtMnYtM2gtM3YtMmgzVjloMnYzaDN2MnoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-not-trusted: url(data:image/svg+xml;base64,PHN2ZyBmaWxsPSJub25lIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI1IDI1Ij4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uMiIgc3Ryb2tlPSIjMzMzMzMzIiBzdHJva2Utd2lkdGg9IjIiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDMgMykiIGQ9Ik0xLjg2MDk0IDExLjQ0MDlDMC44MjY0NDggOC43NzAyNyAwLjg2Mzc3OSA2LjA1NzY0IDEuMjQ5MDcgNC4xOTkzMkMyLjQ4MjA2IDMuOTMzNDcgNC4wODA2OCAzLjQwMzQ3IDUuNjAxMDIgMi44NDQ5QzcuMjM1NDkgMi4yNDQ0IDguODU2NjYgMS41ODE1IDkuOTg3NiAxLjA5NTM5QzExLjA1OTcgMS41ODM0MSAxMi42MDk0IDIuMjQ0NCAxNC4yMTggMi44NDMzOUMxNS43NTAzIDMuNDEzOTQgMTcuMzk5NSAzLjk1MjU4IDE4Ljc1MzkgNC4yMTM4NUMxOS4xMzY0IDYuMDcxNzcgMTkuMTcwOSA4Ljc3NzIyIDE4LjEzOSAxMS40NDA5QzE3LjAzMDMgMTQuMzAzMiAxNC42NjY4IDE3LjE4NDQgOS45OTk5OSAxOC45MzU0QzUuMzMzMTkgMTcuMTg0NCAyLjk2OTY4IDE0LjMwMzIgMS44NjA5NCAxMS40NDA5WiIvPgogICAgPHBhdGggY2xhc3M9ImpwLWljb24yIiBzdHJva2U9IiMzMzMzMzMiIHN0cm9rZS13aWR0aD0iMiIgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoOS4zMTU5MiA5LjMyMDMxKSIgZD0iTTcuMzY4NDIgMEwwIDcuMzY0NzkiLz4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uMiIgc3Ryb2tlPSIjMzMzMzMzIiBzdHJva2Utd2lkdGg9IjIiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDkuMzE1OTIgMTYuNjgzNikgc2NhbGUoMSAtMSkiIGQ9Ik03LjM2ODQyIDBMMCA3LjM2NDc5Ii8+Cjwvc3ZnPgo=);
  --jp-icon-notebook: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8ZyBjbGFzcz0ianAtaWNvbi13YXJuMCBqcC1pY29uLXNlbGVjdGFibGUiIGZpbGw9IiNFRjZDMDAiPgogICAgPHBhdGggZD0iTTE4LjcgMy4zdjE1LjRIMy4zVjMuM2gxNS40bTEuNS0xLjVIMS44djE4LjNoMTguM2wuMS0xOC4zeiIvPgogICAgPHBhdGggZD0iTTE2LjUgMTYuNWwtNS40LTQuMy01LjYgNC4zdi0xMWgxMXoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-numbering: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyOCAyOCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CgkJPHBhdGggZD0iTTQgMTlINlYxOS41SDVWMjAuNUg2VjIxSDRWMjJIN1YxOEg0VjE5Wk01IDEwSDZWNkg0VjdINVYxMFpNNCAxM0g1LjhMNCAxNS4xVjE2SDdWMTVINS4yTDcgMTIuOVYxMkg0VjEzWk05IDdWOUgyM1Y3SDlaTTkgMjFIMjNWMTlIOVYyMVpNOSAxNUgyM1YxM0g5VjE1WiIvPgoJPC9nPgo8L3N2Zz4K);
  --jp-icon-offline-bolt: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCIgd2lkdGg9IjE2Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTEyIDIuMDJjLTUuNTEgMC05Ljk4IDQuNDctOS45OCA5Ljk4czQuNDcgOS45OCA5Ljk4IDkuOTggOS45OC00LjQ3IDkuOTgtOS45OFMxNy41MSAyLjAyIDEyIDIuMDJ6TTExLjQ4IDIwdi02LjI2SDhMMTMgNHY2LjI2aDMuMzVMMTEuNDggMjB6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-palette: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTE4IDEzVjIwSDRWNkg5LjAyQzkuMDcgNS4yOSA5LjI0IDQuNjIgOS41IDRINEMyLjkgNCAyIDQuOSAyIDZWMjBDMiAyMS4xIDIuOSAyMiA0IDIySDE4QzE5LjEgMjIgMjAgMjEuMSAyMCAyMFYxNUwxOCAxM1pNMTkuMyA4Ljg5QzE5Ljc0IDguMTkgMjAgNy4zOCAyMCA2LjVDMjAgNC4wMSAxNy45OSAyIDE1LjUgMkMxMy4wMSAyIDExIDQuMDEgMTEgNi41QzExIDguOTkgMTMuMDEgMTEgMTUuNDkgMTFDMTYuMzcgMTEgMTcuMTkgMTAuNzQgMTcuODggMTAuM0wyMSAxMy40MkwyMi40MiAxMkwxOS4zIDguODlaTTE1LjUgOUMxNC4xMiA5IDEzIDcuODggMTMgNi41QzEzIDUuMTIgMTQuMTIgNCAxNS41IDRDMTYuODggNCAxOCA1LjEyIDE4IDYuNUMxOCA3Ljg4IDE2Ljg4IDkgMTUuNSA5WiIvPgogICAgPHBhdGggZmlsbC1ydWxlPSJldmVub2RkIiBjbGlwLXJ1bGU9ImV2ZW5vZGQiIGQ9Ik00IDZIOS4wMTg5NEM5LjAwNjM5IDYuMTY1MDIgOSA2LjMzMTc2IDkgNi41QzkgOC44MTU3NyAxMC4yMTEgMTAuODQ4NyAxMi4wMzQzIDEySDlWMTRIMTZWMTIuOTgxMUMxNi41NzAzIDEyLjkzNzcgMTcuMTIgMTIuODIwNyAxNy42Mzk2IDEyLjYzOTZMMTggMTNWMjBINFY2Wk04IDhINlYxMEg4VjhaTTYgMTJIOFYxNEg2VjEyWk04IDE2SDZWMThIOFYxNlpNOSAxNkgxNlYxOEg5VjE2WiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-paste: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTE5IDJoLTQuMThDMTQuNC44NCAxMy4zIDAgMTIgMGMtMS4zIDAtMi40Ljg0LTIuODIgMkg1Yy0xLjEgMC0yIC45LTIgMnYxNmMwIDEuMS45IDIgMiAyaDE0YzEuMSAwIDItLjkgMi0yVjRjMC0xLjEtLjktMi0yLTJ6bS03IDBjLjU1IDAgMSAuNDUgMSAxcy0uNDUgMS0xIDEtMS0uNDUtMS0xIC40NS0xIDEtMXptNyAxOEg1VjRoMnYzaDEwVjRoMnYxNnoiLz4KICAgIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-pdf: url(data:image/svg+xml;base64,PHN2ZwogICB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyMiAyMiIgd2lkdGg9IjE2Ij4KICAgIDxwYXRoIHRyYW5zZm9ybT0icm90YXRlKDQ1KSIgY2xhc3M9ImpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iI0ZGMkEyQSIKICAgICAgIGQ9Im0gMjIuMzQ0MzY5LC0zLjAxNjM2NDIgaCA1LjYzODYwNCB2IDEuNTc5MjQzMyBoIC0zLjU0OTIyNyB2IDEuNTA4NjkyOTkgaCAzLjMzNzU3NiBWIDEuNjUwODE1NCBoIC0zLjMzNzU3NiB2IDMuNDM1MjYxMyBoIC0yLjA4OTM3NyB6IG0gLTcuMTM2NDQ0LDEuNTc5MjQzMyB2IDQuOTQzOTU0MyBoIDAuNzQ4OTIgcSAxLjI4MDc2MSwwIDEuOTUzNzAzLC0wLjYzNDk1MzUgMC42NzgzNjksLTAuNjM0OTUzNSAwLjY3ODM2OSwtMS44NDUxNjQxIDAsLTEuMjA0NzgzNTUgLTAuNjcyOTQyLC0xLjgzNDMxMDExIC0wLjY3Mjk0MiwtMC42Mjk1MjY1OSAtMS45NTkxMywtMC42Mjk1MjY1OSB6IG0gLTIuMDg5Mzc3LC0xLjU3OTI0MzMgaCAyLjIwMzM0MyBxIDEuODQ1MTY0LDAgMi43NDYwMzksMC4yNjU5MjA3IDAuOTA2MzAxLDAuMjYwNDkzNyAxLjU1MjEwOCwwLjg5MDAyMDMgMC41Njk4MywwLjU0ODEyMjMgMC44NDY2MDUsMS4yNjQ0ODAwNiAwLjI3Njc3NCwwLjcxNjM1NzgxIDAuMjc2Nzc0LDEuNjIyNjU4OTQgMCwwLjkxNzE1NTEgLTAuMjc2Nzc0LDEuNjM4OTM5OSAtMC4yNzY3NzUsMC43MTYzNTc4IC0wLjg0NjYwNSwxLjI2NDQ4IC0wLjY1MTIzNCwwLjYyOTUyNjYgLTEuNTYyOTYyLDAuODk1NDQ3MyAtMC45MTE3MjgsMC4yNjA0OTM3IC0yLjczNTE4NSwwLjI2MDQ5MzcgaCAtMi4yMDMzNDMgeiBtIC04LjE0NTg1NjUsMCBoIDMuNDY3ODIzIHEgMS41NDY2ODE2LDAgMi4zNzE1Nzg1LDAuNjg5MjIzIDAuODMwMzI0LDAuNjgzNzk2MSAwLjgzMDMyNCwxLjk1MzcwMzE0IDAsMS4yNzUzMzM5NyAtMC44MzAzMjQsMS45NjQ1NTcwNiBRIDkuOTg3MTk2MSwyLjI3NDkxNSA4LjQ0MDUxNDUsMi4yNzQ5MTUgSCA3LjA2MjA2ODQgViA1LjA4NjA3NjcgSCA0Ljk3MjY5MTUgWiBtIDIuMDg5Mzc2OSwxLjUxNDExOTkgdiAyLjI2MzAzOTQzIGggMS4xNTU5NDEgcSAwLjYwNzgxODgsMCAwLjkzODg2MjksLTAuMjkzMDU1NDcgMC4zMzEwNDQxLC0wLjI5ODQ4MjQxIDAuMzMxMDQ0MSwtMC44NDExNzc3MiAwLC0wLjU0MjY5NTMxIC0wLjMzMTA0NDEsLTAuODM1NzUwNzQgLTAuMzMxMDQ0MSwtMC4yOTMwNTU1IC0wLjkzODg2MjksLTAuMjkzMDU1NSB6IgovPgo8L3N2Zz4K);
  --jp-icon-python: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8ZyBjbGFzcz0ianAtaWNvbi1icmFuZDAganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjMEQ0N0ExIj4KICAgIDxwYXRoIGQ9Ik0xMS4xIDYuOVY1LjhINi45YzAtLjUgMC0xLjMuMi0xLjYuNC0uNy44LTEuMSAxLjctMS40IDEuNy0uMyAyLjUtLjMgMy45LS4xIDEgLjEgMS45LjkgMS45IDEuOXY0LjJjMCAuNS0uOSAxLjYtMiAxLjZIOC44Yy0xLjUgMC0yLjQgMS40LTIuNCAyLjh2Mi4ySDQuN0MzLjUgMTUuMSAzIDE0IDMgMTMuMVY5Yy0uMS0xIC42LTIgMS44LTIgMS41LS4xIDYuMy0uMSA2LjMtLjF6Ii8+CiAgICA8cGF0aCBkPSJNMTAuOSAxNS4xdjEuMWg0LjJjMCAuNSAwIDEuMy0uMiAxLjYtLjQuNy0uOCAxLjEtMS43IDEuNC0xLjcuMy0yLjUuMy0zLjkuMS0xLS4xLTEuOS0uOS0xLjktMS45di00LjJjMC0uNS45LTEuNiAyLTEuNmgzLjhjMS41IDAgMi40LTEuNCAyLjQtMi44VjYuNmgxLjdDMTguNSA2LjkgMTkgOCAxOSA4LjlWMTNjMCAxLS43IDIuMS0xLjkgMi4xaC02LjJ6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-r-kernel: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8cGF0aCBjbGFzcz0ianAtaWNvbi1jb250cmFzdDMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjMjE5NkYzIiBkPSJNNC40IDIuNWMxLjItLjEgMi45LS4zIDQuOS0uMyAyLjUgMCA0LjEuNCA1LjIgMS4zIDEgLjcgMS41IDEuOSAxLjUgMy41IDAgMi0xLjQgMy41LTIuOSA0LjEgMS4yLjQgMS43IDEuNiAyLjIgMyAuNiAxLjkgMSAzLjkgMS4zIDQuNmgtMy44Yy0uMy0uNC0uOC0xLjctMS4yLTMuN3MtMS4yLTIuNi0yLjYtMi42aC0uOXY2LjRINC40VjIuNXptMy43IDYuOWgxLjRjMS45IDAgMi45LS45IDIuOS0yLjNzLTEtMi4zLTIuOC0yLjNjLS43IDAtMS4zIDAtMS42LjJ2NC41aC4xdi0uMXoiLz4KPC9zdmc+Cg==);
  --jp-icon-react: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMTUwIDE1MCA1NDEuOSAyOTUuMyI+CiAgPGcgY2xhc3M9ImpwLWljb24tYnJhbmQyIGpwLWljb24tc2VsZWN0YWJsZSIgZmlsbD0iIzYxREFGQiI+CiAgICA8cGF0aCBkPSJNNjY2LjMgMjk2LjVjMC0zMi41LTQwLjctNjMuMy0xMDMuMS04Mi40IDE0LjQtNjMuNiA4LTExNC4yLTIwLjItMTMwLjQtNi41LTMuOC0xNC4xLTUuNi0yMi40LTUuNnYyMi4zYzQuNiAwIDguMy45IDExLjQgMi42IDEzLjYgNy44IDE5LjUgMzcuNSAxNC45IDc1LjctMS4xIDkuNC0yLjkgMTkuMy01LjEgMjkuNC0xOS42LTQuOC00MS04LjUtNjMuNS0xMC45LTEzLjUtMTguNS0yNy41LTM1LjMtNDEuNi01MCAzMi42LTMwLjMgNjMuMi00Ni45IDg0LTQ2LjlWNzhjLTI3LjUgMC02My41IDE5LjYtOTkuOSA1My42LTM2LjQtMzMuOC03Mi40LTUzLjItOTkuOS01My4ydjIyLjNjMjAuNyAwIDUxLjQgMTYuNSA4NCA0Ni42LTE0IDE0LjctMjggMzEuNC00MS4zIDQ5LjktMjIuNiAyLjQtNDQgNi4xLTYzLjYgMTEtMi4zLTEwLTQtMTkuNy01LjItMjktNC43LTM4LjIgMS4xLTY3LjkgMTQuNi03NS44IDMtMS44IDYuOS0yLjYgMTEuNS0yLjZWNzguNWMtOC40IDAtMTYgMS44LTIyLjYgNS42LTI4LjEgMTYuMi0zNC40IDY2LjctMTkuOSAxMzAuMS02Mi4yIDE5LjItMTAyLjcgNDkuOS0xMDIuNyA4Mi4zIDAgMzIuNSA0MC43IDYzLjMgMTAzLjEgODIuNC0xNC40IDYzLjYtOCAxMTQuMiAyMC4yIDEzMC40IDYuNSAzLjggMTQuMSA1LjYgMjIuNSA1LjYgMjcuNSAwIDYzLjUtMTkuNiA5OS45LTUzLjYgMzYuNCAzMy44IDcyLjQgNTMuMiA5OS45IDUzLjIgOC40IDAgMTYtMS44IDIyLjYtNS42IDI4LjEtMTYuMiAzNC40LTY2LjcgMTkuOS0xMzAuMSA2Mi0xOS4xIDEwMi41LTQ5LjkgMTAyLjUtODIuM3ptLTEzMC4yLTY2LjdjLTMuNyAxMi45LTguMyAyNi4yLTEzLjUgMzkuNS00LjEtOC04LjQtMTYtMTMuMS0yNC00LjYtOC05LjUtMTUuOC0xNC40LTIzLjQgMTQuMiAyLjEgMjcuOSA0LjcgNDEgNy45em0tNDUuOCAxMDYuNWMtNy44IDEzLjUtMTUuOCAyNi4zLTI0LjEgMzguMi0xNC45IDEuMy0zMCAyLTQ1LjIgMi0xNS4xIDAtMzAuMi0uNy00NS0xLjktOC4zLTExLjktMTYuNC0yNC42LTI0LjItMzgtNy42LTEzLjEtMTQuNS0yNi40LTIwLjgtMzkuOCA2LjItMTMuNCAxMy4yLTI2LjggMjAuNy0zOS45IDcuOC0xMy41IDE1LjgtMjYuMyAyNC4xLTM4LjIgMTQuOS0xLjMgMzAtMiA0NS4yLTIgMTUuMSAwIDMwLjIuNyA0NSAxLjkgOC4zIDExLjkgMTYuNCAyNC42IDI0LjIgMzggNy42IDEzLjEgMTQuNSAyNi40IDIwLjggMzkuOC02LjMgMTMuNC0xMy4yIDI2LjgtMjAuNyAzOS45em0zMi4zLTEzYzUuNCAxMy40IDEwIDI2LjggMTMuOCAzOS44LTEzLjEgMy4yLTI2LjkgNS45LTQxLjIgOCA0LjktNy43IDkuOC0xNS42IDE0LjQtMjMuNyA0LjYtOCA4LjktMTYuMSAxMy0yNC4xek00MjEuMiA0MzBjLTkuMy05LjYtMTguNi0yMC4zLTI3LjgtMzIgOSAuNCAxOC4yLjcgMjcuNS43IDkuNCAwIDE4LjctLjIgMjcuOC0uNy05IDExLjctMTguMyAyMi40LTI3LjUgMzJ6bS03NC40LTU4LjljLTE0LjItMi4xLTI3LjktNC43LTQxLTcuOSAzLjctMTIuOSA4LjMtMjYuMiAxMy41LTM5LjUgNC4xIDggOC40IDE2IDEzLjEgMjQgNC43IDggOS41IDE1LjggMTQuNCAyMy40ek00MjAuNyAxNjNjOS4zIDkuNiAxOC42IDIwLjMgMjcuOCAzMi05LS40LTE4LjItLjctMjcuNS0uNy05LjQgMC0xOC43LjItMjcuOC43IDktMTEuNyAxOC4zLTIyLjQgMjcuNS0zMnptLTc0IDU4LjljLTQuOSA3LjctOS44IDE1LjYtMTQuNCAyMy43LTQuNiA4LTguOSAxNi0xMyAyNC01LjQtMTMuNC0xMC0yNi44LTEzLjgtMzkuOCAxMy4xLTMuMSAyNi45LTUuOCA0MS4yLTcuOXptLTkwLjUgMTI1LjJjLTM1LjQtMTUuMS01OC4zLTM0LjktNTguMy01MC42IDAtMTUuNyAyMi45LTM1LjYgNTguMy01MC42IDguNi0zLjcgMTgtNyAyNy43LTEwLjEgNS43IDE5LjYgMTMuMiA0MCAyMi41IDYwLjktOS4yIDIwLjgtMTYuNiA0MS4xLTIyLjIgNjAuNi05LjktMy4xLTE5LjMtNi41LTI4LTEwLjJ6TTMxMCA0OTBjLTEzLjYtNy44LTE5LjUtMzcuNS0xNC45LTc1LjcgMS4xLTkuNCAyLjktMTkuMyA1LjEtMjkuNCAxOS42IDQuOCA0MSA4LjUgNjMuNSAxMC45IDEzLjUgMTguNSAyNy41IDM1LjMgNDEuNiA1MC0zMi42IDMwLjMtNjMuMiA0Ni45LTg0IDQ2LjktNC41LS4xLTguMy0xLTExLjMtMi43em0yMzcuMi03Ni4yYzQuNyAzOC4yLTEuMSA2Ny45LTE0LjYgNzUuOC0zIDEuOC02LjkgMi42LTExLjUgMi42LTIwLjcgMC01MS40LTE2LjUtODQtNDYuNiAxNC0xNC43IDI4LTMxLjQgNDEuMy00OS45IDIyLjYtMi40IDQ0LTYuMSA2My42LTExIDIuMyAxMC4xIDQuMSAxOS44IDUuMiAyOS4xem0zOC41LTY2LjdjLTguNiAzLjctMTggNy0yNy43IDEwLjEtNS43LTE5LjYtMTMuMi00MC0yMi41LTYwLjkgOS4yLTIwLjggMTYuNi00MS4xIDIyLjItNjAuNiA5LjkgMy4xIDE5LjMgNi41IDI4LjEgMTAuMiAzNS40IDE1LjEgNTguMyAzNC45IDU4LjMgNTAuNi0uMSAxNS43LTIzIDM1LjYtNTguNCA1MC42ek0zMjAuOCA3OC40eiIvPgogICAgPGNpcmNsZSBjeD0iNDIwLjkiIGN5PSIyOTYuNSIgcj0iNDUuNyIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-redo: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgd2lkdGg9IjE2Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgICA8cGF0aCBkPSJNMCAwaDI0djI0SDB6IiBmaWxsPSJub25lIi8+PHBhdGggZD0iTTE4LjQgMTAuNkMxNi41NSA4Ljk5IDE0LjE1IDggMTEuNSA4Yy00LjY1IDAtOC41OCAzLjAzLTkuOTYgNy4yMkwzLjkgMTZjMS4wNS0zLjE5IDQuMDUtNS41IDcuNi01LjUgMS45NSAwIDMuNzMuNzIgNS4xMiAxLjg4TDEzIDE2aDlWN2wtMy42IDMuNnoiLz4KICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-refresh: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDE4IDE4Ij4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTkgMTMuNWMtMi40OSAwLTQuNS0yLjAxLTQuNS00LjVTNi41MSA0LjUgOSA0LjVjMS4yNCAwIDIuMzYuNTIgMy4xNyAxLjMzTDEwIDhoNVYzbC0xLjc2IDEuNzZDMTIuMTUgMy42OCAxMC42NiAzIDkgMyA1LjY5IDMgMy4wMSA1LjY5IDMuMDEgOVM1LjY5IDE1IDkgMTVjMi45NyAwIDUuNDMtMi4xNiA1LjktNWgtMS41MmMtLjQ2IDItMi4yNCAzLjUtNC4zOCAzLjV6Ii8+CiAgICA8L2c+Cjwvc3ZnPgo=);
  --jp-icon-regex: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIwIDIwIj4KICA8ZyBjbGFzcz0ianAtaWNvbjIiIGZpbGw9IiM0MTQxNDEiPgogICAgPHJlY3QgeD0iMiIgeT0iMiIgd2lkdGg9IjE2IiBoZWlnaHQ9IjE2Ii8+CiAgPC9nPgoKICA8ZyBjbGFzcz0ianAtaWNvbi1hY2NlbnQyIiBmaWxsPSIjRkZGIj4KICAgIDxjaXJjbGUgY2xhc3M9InN0MiIgY3g9IjUuNSIgY3k9IjE0LjUiIHI9IjEuNSIvPgogICAgPHJlY3QgeD0iMTIiIHk9IjQiIGNsYXNzPSJzdDIiIHdpZHRoPSIxIiBoZWlnaHQ9IjgiLz4KICAgIDxyZWN0IHg9IjguNSIgeT0iNy41IiB0cmFuc2Zvcm09Im1hdHJpeCgwLjg2NiAtMC41IDAuNSAwLjg2NiAtMi4zMjU1IDcuMzIxOSkiIGNsYXNzPSJzdDIiIHdpZHRoPSI4IiBoZWlnaHQ9IjEiLz4KICAgIDxyZWN0IHg9IjEyIiB5PSI0IiB0cmFuc2Zvcm09Im1hdHJpeCgwLjUgLTAuODY2IDAuODY2IDAuNSAtMC42Nzc5IDE0LjgyNTIpIiBjbGFzcz0ic3QyIiB3aWR0aD0iMSIgaGVpZ2h0PSI4Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-run: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTggNXYxNGwxMS03eiIvPgogICAgPC9nPgo8L3N2Zz4K);
  --jp-icon-running: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDUxMiA1MTIiPgogIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICA8cGF0aCBkPSJNMjU2IDhDMTE5IDggOCAxMTkgOCAyNTZzMTExIDI0OCAyNDggMjQ4IDI0OC0xMTEgMjQ4LTI0OFMzOTMgOCAyNTYgOHptOTYgMzI4YzAgOC44LTcuMiAxNi0xNiAxNkgxNzZjLTguOCAwLTE2LTcuMi0xNi0xNlYxNzZjMC04LjggNy4yLTE2IDE2LTE2aDE2MGM4LjggMCAxNiA3LjIgMTYgMTZ2MTYweiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-save: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTE3IDNINWMtMS4xMSAwLTIgLjktMiAydjE0YzAgMS4xLjg5IDIgMiAyaDE0YzEuMSAwIDItLjkgMi0yVjdsLTQtNHptLTUgMTZjLTEuNjYgMC0zLTEuMzQtMy0zczEuMzQtMyAzLTMgMyAxLjM0IDMgMy0xLjM0IDMtMyAzem0zLTEwSDVWNWgxMHY0eiIvPgogICAgPC9nPgo8L3N2Zz4K);
  --jp-icon-search: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMTggMTgiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTEyLjEsMTAuOWgtMC43bC0wLjItMC4yYzAuOC0wLjksMS4zLTIuMiwxLjMtMy41YzAtMy0yLjQtNS40LTUuNC01LjRTMS44LDQuMiwxLjgsNy4xczIuNCw1LjQsNS40LDUuNCBjMS4zLDAsMi41LTAuNSwzLjUtMS4zbDAuMiwwLjJ2MC43bDQuMSw0LjFsMS4yLTEuMkwxMi4xLDEwLjl6IE03LjEsMTAuOWMtMi4xLDAtMy43LTEuNy0zLjctMy43czEuNy0zLjcsMy43LTMuN3MzLjcsMS43LDMuNywzLjcgUzkuMiwxMC45LDcuMSwxMC45eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-settings: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMTkuNDMgMTIuOThjLjA0LS4zMi4wNy0uNjQuMDctLjk4cy0uMDMtLjY2LS4wNy0uOThsMi4xMS0xLjY1Yy4xOS0uMTUuMjQtLjQyLjEyLS42NGwtMi0zLjQ2Yy0uMTItLjIyLS4zOS0uMy0uNjEtLjIybC0yLjQ5IDFjLS41Mi0uNC0xLjA4LS43My0xLjY5LS45OGwtLjM4LTIuNjVBLjQ4OC40ODggMCAwMDE0IDJoLTRjLS4yNSAwLS40Ni4xOC0uNDkuNDJsLS4zOCAyLjY1Yy0uNjEuMjUtMS4xNy41OS0xLjY5Ljk4bC0yLjQ5LTFjLS4yMy0uMDktLjQ5IDAtLjYxLjIybC0yIDMuNDZjLS4xMy4yMi0uMDcuNDkuMTIuNjRsMi4xMSAxLjY1Yy0uMDQuMzItLjA3LjY1LS4wNy45OHMuMDMuNjYuMDcuOThsLTIuMTEgMS42NWMtLjE5LjE1LS4yNC40Mi0uMTIuNjRsMiAzLjQ2Yy4xMi4yMi4zOS4zLjYxLjIybDIuNDktMWMuNTIuNCAxLjA4LjczIDEuNjkuOThsLjM4IDIuNjVjLjAzLjI0LjI0LjQyLjQ5LjQyaDRjLjI1IDAgLjQ2LS4xOC40OS0uNDJsLjM4LTIuNjVjLjYxLS4yNSAxLjE3LS41OSAxLjY5LS45OGwyLjQ5IDFjLjIzLjA5LjQ5IDAgLjYxLS4yMmwyLTMuNDZjLjEyLS4yMi4wNy0uNDktLjEyLS42NGwtMi4xMS0xLjY1ek0xMiAxNS41Yy0xLjkzIDAtMy41LTEuNTctMy41LTMuNXMxLjU3LTMuNSAzLjUtMy41IDMuNSAxLjU3IDMuNSAzLjUtMS41NyAzLjUtMy41IDMuNXoiLz4KPC9zdmc+Cg==);
  --jp-icon-spreadsheet: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8cGF0aCBjbGFzcz0ianAtaWNvbi1jb250cmFzdDEganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNENBRjUwIiBkPSJNMi4yIDIuMnYxNy42aDE3LjZWMi4ySDIuMnptMTUuNCA3LjdoLTUuNVY0LjRoNS41djUuNXpNOS45IDQuNHY1LjVINC40VjQuNGg1LjV6bS01LjUgNy43aDUuNXY1LjVINC40di01LjV6bTcuNyA1LjV2LTUuNWg1LjV2NS41aC01LjV6Ii8+Cjwvc3ZnPgo=);
  --jp-icon-stop: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTAgMGgyNHYyNEgweiIgZmlsbD0ibm9uZSIvPgogICAgICAgIDxwYXRoIGQ9Ik02IDZoMTJ2MTJINnoiLz4KICAgIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-tab: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTIxIDNIM2MtMS4xIDAtMiAuOS0yIDJ2MTRjMCAxLjEuOSAyIDIgMmgxOGMxLjEgMCAyLS45IDItMlY1YzAtMS4xLS45LTItMi0yem0wIDE2SDNWNWgxMHY0aDh2MTB6Ii8+CiAgPC9nPgo8L3N2Zz4K);
  --jp-icon-table-rows: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTAgMGgyNHYyNEgweiIgZmlsbD0ibm9uZSIvPgogICAgICAgIDxwYXRoIGQ9Ik0yMSw4SDNWNGgxOFY4eiBNMjEsMTBIM3Y0aDE4VjEweiBNMjEsMTZIM3Y0aDE4VjE2eiIvPgogICAgPC9nPgo8L3N2Zz4=);
  --jp-icon-tag: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjgiIGhlaWdodD0iMjgiIHZpZXdCb3g9IjAgMCA0MyAyOCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CgkJPHBhdGggZD0iTTI4LjgzMzIgMTIuMzM0TDMyLjk5OTggMTYuNTAwN0wzNy4xNjY1IDEyLjMzNEgyOC44MzMyWiIvPgoJCTxwYXRoIGQ9Ik0xNi4yMDk1IDIxLjYxMDRDMTUuNjg3MyAyMi4xMjk5IDE0Ljg0NDMgMjIuMTI5OSAxNC4zMjQ4IDIxLjYxMDRMNi45ODI5IDE0LjcyNDVDNi41NzI0IDE0LjMzOTQgNi4wODMxMyAxMy42MDk4IDYuMDQ3ODYgMTMuMDQ4MkM1Ljk1MzQ3IDExLjUyODggNi4wMjAwMiA4LjYxOTQ0IDYuMDY2MjEgNy4wNzY5NUM2LjA4MjgxIDYuNTE0NzcgNi41NTU0OCA2LjA0MzQ3IDcuMTE4MDQgNi4wMzA1NUM5LjA4ODYzIDUuOTg0NzMgMTMuMjYzOCA1LjkzNTc5IDEzLjY1MTggNi4zMjQyNUwyMS43MzY5IDEzLjYzOUMyMi4yNTYgMTQuMTU4NSAyMS43ODUxIDE1LjQ3MjQgMjEuMjYyIDE1Ljk5NDZMMTYuMjA5NSAyMS42MTA0Wk05Ljc3NTg1IDguMjY1QzkuMzM1NTEgNy44MjU2NiA4LjYyMzUxIDcuODI1NjYgOC4xODI4IDguMjY1QzcuNzQzNDYgOC43MDU3MSA3Ljc0MzQ2IDkuNDE3MzMgOC4xODI4IDkuODU2NjdDOC42MjM4MiAxMC4yOTY0IDkuMzM1ODIgMTAuMjk2NCA5Ljc3NTg1IDkuODU2NjdDMTAuMjE1NiA5LjQxNzMzIDEwLjIxNTYgOC43MDUzMyA5Ljc3NTg1IDguMjY1WiIvPgoJPC9nPgo8L3N2Zz4K);
  --jp-icon-terminal: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0IiA+CiAgICA8cmVjdCBjbGFzcz0ianAtaWNvbjIganAtaWNvbi1zZWxlY3RhYmxlIiB3aWR0aD0iMjAiIGhlaWdodD0iMjAiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDIgMikiIGZpbGw9IiMzMzMzMzMiLz4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uLWFjY2VudDIganAtaWNvbi1zZWxlY3RhYmxlLWludmVyc2UiIGQ9Ik01LjA1NjY0IDguNzYxNzJDNS4wNTY2NCA4LjU5NzY2IDUuMDMxMjUgOC40NTMxMiA0Ljk4MDQ3IDguMzI4MTJDNC45MzM1OSA4LjE5OTIyIDQuODU1NDcgOC4wODIwMyA0Ljc0NjA5IDcuOTc2NTZDNC42NDA2MiA3Ljg3MTA5IDQuNSA3Ljc3NTM5IDQuMzI0MjIgNy42ODk0NUM0LjE1MjM0IDcuNTk5NjEgMy45NDMzNiA3LjUxMTcyIDMuNjk3MjcgNy40MjU3OEMzLjMwMjczIDcuMjg1MTYgMi45NDMzNiA3LjEzNjcyIDIuNjE5MTQgNi45ODA0N0MyLjI5NDkyIDYuODI0MjIgMi4wMTc1OCA2LjY0MjU4IDEuNzg3MTEgNi40MzU1NUMxLjU2MDU1IDYuMjI4NTIgMS4zODQ3NyA1Ljk4ODI4IDEuMjU5NzcgNS43MTQ4NEMxLjEzNDc3IDUuNDM3NSAxLjA3MjI3IDUuMTA5MzggMS4wNzIyNyA0LjczMDQ3QzEuMDcyMjcgNC4zOTg0NCAxLjEyODkxIDQuMDk1NyAxLjI0MjE5IDMuODIyMjdDMS4zNTU0NyAzLjU0NDkyIDEuNTE1NjIgMy4zMDQ2OSAxLjcyMjY2IDMuMTAxNTZDMS45Mjk2OSAyLjg5ODQ0IDIuMTc5NjkgMi43MzQzNyAyLjQ3MjY2IDIuNjA5MzhDMi43NjU2MiAyLjQ4NDM4IDMuMDkxOCAyLjQwNDMgMy40NTExNyAyLjM2OTE0VjEuMTA5MzhINC4zODg2N1YyLjM4MDg2QzQuNzQwMjMgMi40Mjc3MyA1LjA1NjY0IDIuNTIzNDQgNS4zMzc4OSAyLjY2Nzk3QzUuNjE5MTQgMi44MTI1IDUuODU3NDIgMy4wMDE5NSA2LjA1MjczIDMuMjM2MzNDNi4yNTE5NSAzLjQ2NjggNi40MDQzIDMuNzQwMjMgNi41MDk3NyA0LjA1NjY0QzYuNjE5MTQgNC4zNjkxNCA2LjY3MzgzIDQuNzIwNyA2LjY3MzgzIDUuMTExMzNINS4wNDQ5MkM1LjA0NDkyIDQuNjM4NjcgNC45Mzc1IDQuMjgxMjUgNC43MjI2NiA0LjAzOTA2QzQuNTA3ODEgMy43OTI5NyA0LjIxNjggMy42Njk5MiAzLjg0OTYxIDMuNjY5OTJDMy42NTAzOSAzLjY2OTkyIDMuNDc2NTYgMy42OTcyNyAzLjMyODEyIDMuNzUxOTVDMy4xODM1OSAzLjgwMjczIDMuMDY0NDUgMy44NzY5NSAyLjk3MDcgMy45NzQ2MUMyLjg3Njk1IDQuMDY4MzYgMi44MDY2NCA0LjE3OTY5IDIuNzU5NzcgNC4zMDg1OUMyLjcxNjggNC40Mzc1IDIuNjk1MzEgNC41NzgxMiAyLjY5NTMxIDQuNzMwNDdDMi42OTUzMSA0Ljg4MjgxIDIuNzE2OCA1LjAxOTUzIDIuNzU5NzcgNS4xNDA2MkMyLjgwNjY0IDUuMjU3ODEgMi44ODI4MSA1LjM2NzE5IDIuOTg4MjggNS40Njg3NUMzLjA5NzY2IDUuNTcwMzEgMy4yNDAyMyA1LjY2Nzk3IDMuNDE2MDIgNS43NjE3MkMzLjU5MTggNS44NTE1NiAzLjgxMDU1IDUuOTQzMzYgNC4wNzIyNyA2LjAzNzExQzQuNDY2OCA2LjE4NTU1IDQuODI0MjIgNi4zMzk4NCA1LjE0NDUzIDYuNUM1LjQ2NDg0IDYuNjU2MjUgNS43MzgyOCA2LjgzOTg0IDUuOTY0ODQgNy4wNTA3OEM2LjE5NTMxIDcuMjU3ODEgNi4zNzEwOSA3LjUgNi40OTIxOSA3Ljc3NzM0QzYuNjE3MTkgOC4wNTA3OCA2LjY3OTY5IDguMzc1IDYuNjc5NjkgOC43NUM2LjY3OTY5IDkuMDkzNzUgNi42MjMwNSA5LjQwNDMgNi41MDk3NyA5LjY4MTY0QzYuMzk2NDggOS45NTUwOCA2LjIzNDM4IDEwLjE5MTQgNi4wMjM0NCAxMC4zOTA2QzUuODEyNSAxMC41ODk4IDUuNTU4NTkgMTAuNzUgNS4yNjE3MiAxMC44NzExQzQuOTY0ODQgMTAuOTg4MyA0LjYzMjgxIDExLjA2NDUgNC4yNjU2MiAxMS4wOTk2VjEyLjI0OEgzLjMzMzk4VjExLjA5OTZDMy4wMDE5NSAxMS4wNjg0IDIuNjc5NjkgMTAuOTk2MSAyLjM2NzE5IDEwLjg4MjhDMi4wNTQ2OSAxMC43NjU2IDEuNzc3MzQgMTAuNTk3NyAxLjUzNTE2IDEwLjM3ODlDMS4yOTY4OCAxMC4xNjAyIDEuMTA1NDcgOS44ODQ3NyAwLjk2MDkzOCA5LjU1MjczQzAuODE2NDA2IDkuMjE2OCAwLjc0NDE0MSA4LjgxNDQ1IDAuNzQ0MTQxIDguMzQ1N0gyLjM3ODkxQzIuMzc4OTEgOC42MjY5NSAyLjQxOTkyIDguODYzMjggMi41MDE5NSA5LjA1NDY5QzIuNTgzOTggOS4yNDIxOSAyLjY4OTQ1IDkuMzkyNTggMi44MTgzNiA5LjUwNTg2QzIuOTUxMTcgOS42MTUyMyAzLjEwMTU2IDkuNjkzMzYgMy4yNjk1MyA5Ljc0MDIzQzMuNDM3NSA5Ljc4NzExIDMuNjA5MzggOS44MTA1NSAzLjc4NTE2IDkuODEwNTVDNC4yMDMxMiA5LjgxMDU1IDQuNTE5NTMgOS43MTI4OSA0LjczNDM4IDkuNTE3NThDNC45NDkyMiA5LjMyMjI3IDUuMDU2NjQgOS4wNzAzMSA1LjA1NjY0IDguNzYxNzJaTTEzLjQxOCAxMi4yNzE1SDguMDc0MjJWMTFIMTMuNDE4VjEyLjI3MTVaIiB0cmFuc2Zvcm09InRyYW5zbGF0ZSgzLjk1MjY0IDYpIiBmaWxsPSJ3aGl0ZSIvPgo8L3N2Zz4K);
  --jp-icon-text-editor: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIiBkPSJNMTUgMTVIM3YyaDEydi0yem0wLThIM3YyaDEyVjd6TTMgMTNoMTh2LTJIM3Yyem0wIDhoMTh2LTJIM3Yyek0zIDN2MmgxOFYzSDN6Ii8+Cjwvc3ZnPgo=);
  --jp-icon-toc: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8ZyBjbGFzcz0ianAtaWNvbjMganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjNjE2MTYxIj4KICAgIDxwYXRoIGQ9Ik03LDVIMjFWN0g3VjVNNywxM1YxMUgyMVYxM0g3TTQsNC41QTEuNSwxLjUgMCAwLDEgNS41LDZBMS41LDEuNSAwIDAsMSA0LDcuNUExLjUsMS41IDAgMCwxIDIuNSw2QTEuNSwxLjUgMCAwLDEgNCw0LjVNNCwxMC41QTEuNSwxLjUgMCAwLDEgNS41LDEyQTEuNSwxLjUgMCAwLDEgNCwxMy41QTEuNSwxLjUgMCAwLDEgMi41LDEyQTEuNSwxLjUgMCAwLDEgNCwxMC41TTcsMTlWMTdIMjFWMTlIN000LDE2LjVBMS41LDEuNSAwIDAsMSA1LjUsMThBMS41LDEuNSAwIDAsMSA0LDE5LjVBMS41LDEuNSAwIDAsMSAyLjUsMThBMS41LDEuNSAwIDAsMSA0LDE2LjVaIiAvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-tree-view: url(data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjI0IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICAgIDxnIGNsYXNzPSJqcC1pY29uMyIgZmlsbD0iIzYxNjE2MSI+CiAgICAgICAgPHBhdGggZD0iTTAgMGgyNHYyNEgweiIgZmlsbD0ibm9uZSIvPgogICAgICAgIDxwYXRoIGQ9Ik0yMiAxMVYzaC03djNIOVYzSDJ2OGg3VjhoMnYxMGg0djNoN3YtOGgtN3YzaC0yVjhoMnYzeiIvPgogICAgPC9nPgo8L3N2Zz4=);
  --jp-icon-trusted: url(data:image/svg+xml;base64,PHN2ZyBmaWxsPSJub25lIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDI0IDI1Ij4KICAgIDxwYXRoIGNsYXNzPSJqcC1pY29uMiIgc3Ryb2tlPSIjMzMzMzMzIiBzdHJva2Utd2lkdGg9IjIiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDIgMykiIGQ9Ik0xLjg2MDk0IDExLjQ0MDlDMC44MjY0NDggOC43NzAyNyAwLjg2Mzc3OSA2LjA1NzY0IDEuMjQ5MDcgNC4xOTkzMkMyLjQ4MjA2IDMuOTMzNDcgNC4wODA2OCAzLjQwMzQ3IDUuNjAxMDIgMi44NDQ5QzcuMjM1NDkgMi4yNDQ0IDguODU2NjYgMS41ODE1IDkuOTg3NiAxLjA5NTM5QzExLjA1OTcgMS41ODM0MSAxMi42MDk0IDIuMjQ0NCAxNC4yMTggMi44NDMzOUMxNS43NTAzIDMuNDEzOTQgMTcuMzk5NSAzLjk1MjU4IDE4Ljc1MzkgNC4yMTM4NUMxOS4xMzY0IDYuMDcxNzcgMTkuMTcwOSA4Ljc3NzIyIDE4LjEzOSAxMS40NDA5QzE3LjAzMDMgMTQuMzAzMiAxNC42NjY4IDE3LjE4NDQgOS45OTk5OSAxOC45MzU0QzUuMzMzMiAxNy4xODQ0IDIuOTY5NjggMTQuMzAzMiAxLjg2MDk0IDExLjQ0MDlaIi8+CiAgICA8cGF0aCBjbGFzcz0ianAtaWNvbjIiIGZpbGw9IiMzMzMzMzMiIHN0cm9rZT0iIzMzMzMzMyIgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoOCA5Ljg2NzE5KSIgZD0iTTIuODYwMTUgNC44NjUzNUwwLjcyNjU0OSAyLjk5OTU5TDAgMy42MzA0NUwyLjg2MDE1IDYuMTMxNTdMOCAwLjYzMDg3Mkw3LjI3ODU3IDBMMi44NjAxNSA0Ljg2NTM1WiIvPgo8L3N2Zz4K);
  --jp-icon-undo: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHdpZHRoPSIxNiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8ZyBjbGFzcz0ianAtaWNvbjMiIGZpbGw9IiM2MTYxNjEiPgogICAgPHBhdGggZD0iTTEyLjUgOGMtMi42NSAwLTUuMDUuOTktNi45IDIuNkwyIDd2OWg5bC0zLjYyLTMuNjJjMS4zOS0xLjE2IDMuMTYtMS44OCA1LjEyLTEuODggMy41NCAwIDYuNTUgMi4zMSA3LjYgNS41bDIuMzctLjc4QzIxLjA4IDExLjAzIDE3LjE1IDggMTIuNSA4eiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-vega: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8ZyBjbGFzcz0ianAtaWNvbjEganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjMjEyMTIxIj4KICAgIDxwYXRoIGQ9Ik0xMC42IDUuNGwyLjItMy4ySDIuMnY3LjNsNC02LjZ6Ii8+CiAgICA8cGF0aCBkPSJNMTUuOCAyLjJsLTQuNCA2LjZMNyA2LjNsLTQuOCA4djUuNWgxNy42VjIuMmgtNHptLTcgMTUuNEg1LjV2LTQuNGgzLjN2NC40em00LjQgMEg5LjhWOS44aDMuNHY3Ljh6bTQuNCAwaC0zLjRWNi41aDMuNHYxMS4xeiIvPgogIDwvZz4KPC9zdmc+Cg==);
  --jp-icon-yaml: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxNiIgdmlld0JveD0iMCAwIDIyIDIyIj4KICA8ZyBjbGFzcz0ianAtaWNvbi1jb250cmFzdDIganAtaWNvbi1zZWxlY3RhYmxlIiBmaWxsPSIjRDgxQjYwIj4KICAgIDxwYXRoIGQ9Ik03LjIgMTguNnYtNS40TDMgNS42aDMuM2wxLjQgMy4xYy4zLjkuNiAxLjYgMSAyLjUuMy0uOC42LTEuNiAxLTIuNWwxLjQtMy4xaDMuNGwtNC40IDcuNnY1LjVsLTIuOS0uMXoiLz4KICAgIDxjaXJjbGUgY2xhc3M9InN0MCIgY3g9IjE3LjYiIGN5PSIxNi41IiByPSIyLjEiLz4KICAgIDxjaXJjbGUgY2xhc3M9InN0MCIgY3g9IjE3LjYiIGN5PSIxMSIgcj0iMi4xIi8+CiAgPC9nPgo8L3N2Zz4K);
}

/* Icon CSS class declarations */

.jp-AddIcon {
  background-image: var(--jp-icon-add);
}
.jp-BugIcon {
  background-image: var(--jp-icon-bug);
}
.jp-BuildIcon {
  background-image: var(--jp-icon-build);
}
.jp-CaretDownEmptyIcon {
  background-image: var(--jp-icon-caret-down-empty);
}
.jp-CaretDownEmptyThinIcon {
  background-image: var(--jp-icon-caret-down-empty-thin);
}
.jp-CaretDownIcon {
  background-image: var(--jp-icon-caret-down);
}
.jp-CaretLeftIcon {
  background-image: var(--jp-icon-caret-left);
}
.jp-CaretRightIcon {
  background-image: var(--jp-icon-caret-right);
}
.jp-CaretUpEmptyThinIcon {
  background-image: var(--jp-icon-caret-up-empty-thin);
}
.jp-CaretUpIcon {
  background-image: var(--jp-icon-caret-up);
}
.jp-CaseSensitiveIcon {
  background-image: var(--jp-icon-case-sensitive);
}
.jp-CheckIcon {
  background-image: var(--jp-icon-check);
}
.jp-CircleEmptyIcon {
  background-image: var(--jp-icon-circle-empty);
}
.jp-CircleIcon {
  background-image: var(--jp-icon-circle);
}
.jp-ClearIcon {
  background-image: var(--jp-icon-clear);
}
.jp-CloseIcon {
  background-image: var(--jp-icon-close);
}
.jp-CodeIcon {
  background-image: var(--jp-icon-code);
}
.jp-ConsoleIcon {
  background-image: var(--jp-icon-console);
}
.jp-CopyIcon {
  background-image: var(--jp-icon-copy);
}
.jp-CopyrightIcon {
  background-image: var(--jp-icon-copyright);
}
.jp-CutIcon {
  background-image: var(--jp-icon-cut);
}
.jp-DownloadIcon {
  background-image: var(--jp-icon-download);
}
.jp-EditIcon {
  background-image: var(--jp-icon-edit);
}
.jp-EllipsesIcon {
  background-image: var(--jp-icon-ellipses);
}
.jp-ExtensionIcon {
  background-image: var(--jp-icon-extension);
}
.jp-FastForwardIcon {
  background-image: var(--jp-icon-fast-forward);
}
.jp-FileIcon {
  background-image: var(--jp-icon-file);
}
.jp-FileUploadIcon {
  background-image: var(--jp-icon-file-upload);
}
.jp-FilterListIcon {
  background-image: var(--jp-icon-filter-list);
}
.jp-FolderIcon {
  background-image: var(--jp-icon-folder);
}
.jp-Html5Icon {
  background-image: var(--jp-icon-html5);
}
.jp-ImageIcon {
  background-image: var(--jp-icon-image);
}
.jp-InspectorIcon {
  background-image: var(--jp-icon-inspector);
}
.jp-JsonIcon {
  background-image: var(--jp-icon-json);
}
.jp-JuliaIcon {
  background-image: var(--jp-icon-julia);
}
.jp-JupyterFaviconIcon {
  background-image: var(--jp-icon-jupyter-favicon);
}
.jp-JupyterIcon {
  background-image: var(--jp-icon-jupyter);
}
.jp-JupyterlabWordmarkIcon {
  background-image: var(--jp-icon-jupyterlab-wordmark);
}
.jp-KernelIcon {
  background-image: var(--jp-icon-kernel);
}
.jp-KeyboardIcon {
  background-image: var(--jp-icon-keyboard);
}
.jp-LauncherIcon {
  background-image: var(--jp-icon-launcher);
}
.jp-LineFormIcon {
  background-image: var(--jp-icon-line-form);
}
.jp-LinkIcon {
  background-image: var(--jp-icon-link);
}
.jp-ListIcon {
  background-image: var(--jp-icon-list);
}
.jp-ListingsInfoIcon {
  background-image: var(--jp-icon-listings-info);
}
.jp-MarkdownIcon {
  background-image: var(--jp-icon-markdown);
}
.jp-NewFolderIcon {
  background-image: var(--jp-icon-new-folder);
}
.jp-NotTrustedIcon {
  background-image: var(--jp-icon-not-trusted);
}
.jp-NotebookIcon {
  background-image: var(--jp-icon-notebook);
}
.jp-NumberingIcon {
  background-image: var(--jp-icon-numbering);
}
.jp-OfflineBoltIcon {
  background-image: var(--jp-icon-offline-bolt);
}
.jp-PaletteIcon {
  background-image: var(--jp-icon-palette);
}
.jp-PasteIcon {
  background-image: var(--jp-icon-paste);
}
.jp-PdfIcon {
  background-image: var(--jp-icon-pdf);
}
.jp-PythonIcon {
  background-image: var(--jp-icon-python);
}
.jp-RKernelIcon {
  background-image: var(--jp-icon-r-kernel);
}
.jp-ReactIcon {
  background-image: var(--jp-icon-react);
}
.jp-RedoIcon {
  background-image: var(--jp-icon-redo);
}
.jp-RefreshIcon {
  background-image: var(--jp-icon-refresh);
}
.jp-RegexIcon {
  background-image: var(--jp-icon-regex);
}
.jp-RunIcon {
  background-image: var(--jp-icon-run);
}
.jp-RunningIcon {
  background-image: var(--jp-icon-running);
}
.jp-SaveIcon {
  background-image: var(--jp-icon-save);
}
.jp-SearchIcon {
  background-image: var(--jp-icon-search);
}
.jp-SettingsIcon {
  background-image: var(--jp-icon-settings);
}
.jp-SpreadsheetIcon {
  background-image: var(--jp-icon-spreadsheet);
}
.jp-StopIcon {
  background-image: var(--jp-icon-stop);
}
.jp-TabIcon {
  background-image: var(--jp-icon-tab);
}
.jp-TableRowsIcon {
  background-image: var(--jp-icon-table-rows);
}
.jp-TagIcon {
  background-image: var(--jp-icon-tag);
}
.jp-TerminalIcon {
  background-image: var(--jp-icon-terminal);
}
.jp-TextEditorIcon {
  background-image: var(--jp-icon-text-editor);
}
.jp-TocIcon {
  background-image: var(--jp-icon-toc);
}
.jp-TreeViewIcon {
  background-image: var(--jp-icon-tree-view);
}
.jp-TrustedIcon {
  background-image: var(--jp-icon-trusted);
}
.jp-UndoIcon {
  background-image: var(--jp-icon-undo);
}
.jp-VegaIcon {
  background-image: var(--jp-icon-vega);
}
.jp-YamlIcon {
  background-image: var(--jp-icon-yaml);
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/**
 * (DEPRECATED) Support for consuming icons as CSS background images
 */

.jp-Icon,
.jp-MaterialIcon {
  background-position: center;
  background-repeat: no-repeat;
  background-size: 16px;
  min-width: 16px;
  min-height: 16px;
}

.jp-Icon-cover {
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

/**
 * (DEPRECATED) Support for specific CSS icon sizes
 */

.jp-Icon-16 {
  background-size: 16px;
  min-width: 16px;
  min-height: 16px;
}

.jp-Icon-18 {
  background-size: 18px;
  min-width: 18px;
  min-height: 18px;
}

.jp-Icon-20 {
  background-size: 20px;
  min-width: 20px;
  min-height: 20px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/**
 * Support for icons as inline SVG HTMLElements
 */

/* recolor the primary elements of an icon */
.jp-icon0[fill] {
  fill: var(--jp-inverse-layout-color0);
}
.jp-icon1[fill] {
  fill: var(--jp-inverse-layout-color1);
}
.jp-icon2[fill] {
  fill: var(--jp-inverse-layout-color2);
}
.jp-icon3[fill] {
  fill: var(--jp-inverse-layout-color3);
}
.jp-icon4[fill] {
  fill: var(--jp-inverse-layout-color4);
}

.jp-icon0[stroke] {
  stroke: var(--jp-inverse-layout-color0);
}
.jp-icon1[stroke] {
  stroke: var(--jp-inverse-layout-color1);
}
.jp-icon2[stroke] {
  stroke: var(--jp-inverse-layout-color2);
}
.jp-icon3[stroke] {
  stroke: var(--jp-inverse-layout-color3);
}
.jp-icon4[stroke] {
  stroke: var(--jp-inverse-layout-color4);
}
/* recolor the accent elements of an icon */
.jp-icon-accent0[fill] {
  fill: var(--jp-layout-color0);
}
.jp-icon-accent1[fill] {
  fill: var(--jp-layout-color1);
}
.jp-icon-accent2[fill] {
  fill: var(--jp-layout-color2);
}
.jp-icon-accent3[fill] {
  fill: var(--jp-layout-color3);
}
.jp-icon-accent4[fill] {
  fill: var(--jp-layout-color4);
}

.jp-icon-accent0[stroke] {
  stroke: var(--jp-layout-color0);
}
.jp-icon-accent1[stroke] {
  stroke: var(--jp-layout-color1);
}
.jp-icon-accent2[stroke] {
  stroke: var(--jp-layout-color2);
}
.jp-icon-accent3[stroke] {
  stroke: var(--jp-layout-color3);
}
.jp-icon-accent4[stroke] {
  stroke: var(--jp-layout-color4);
}
/* set the color of an icon to transparent */
.jp-icon-none[fill] {
  fill: none;
}

.jp-icon-none[stroke] {
  stroke: none;
}
/* brand icon colors. Same for light and dark */
.jp-icon-brand0[fill] {
  fill: var(--jp-brand-color0);
}
.jp-icon-brand1[fill] {
  fill: var(--jp-brand-color1);
}
.jp-icon-brand2[fill] {
  fill: var(--jp-brand-color2);
}
.jp-icon-brand3[fill] {
  fill: var(--jp-brand-color3);
}
.jp-icon-brand4[fill] {
  fill: var(--jp-brand-color4);
}

.jp-icon-brand0[stroke] {
  stroke: var(--jp-brand-color0);
}
.jp-icon-brand1[stroke] {
  stroke: var(--jp-brand-color1);
}
.jp-icon-brand2[stroke] {
  stroke: var(--jp-brand-color2);
}
.jp-icon-brand3[stroke] {
  stroke: var(--jp-brand-color3);
}
.jp-icon-brand4[stroke] {
  stroke: var(--jp-brand-color4);
}
/* warn icon colors. Same for light and dark */
.jp-icon-warn0[fill] {
  fill: var(--jp-warn-color0);
}
.jp-icon-warn1[fill] {
  fill: var(--jp-warn-color1);
}
.jp-icon-warn2[fill] {
  fill: var(--jp-warn-color2);
}
.jp-icon-warn3[fill] {
  fill: var(--jp-warn-color3);
}

.jp-icon-warn0[stroke] {
  stroke: var(--jp-warn-color0);
}
.jp-icon-warn1[stroke] {
  stroke: var(--jp-warn-color1);
}
.jp-icon-warn2[stroke] {
  stroke: var(--jp-warn-color2);
}
.jp-icon-warn3[stroke] {
  stroke: var(--jp-warn-color3);
}
/* icon colors that contrast well with each other and most backgrounds */
.jp-icon-contrast0[fill] {
  fill: var(--jp-icon-contrast-color0);
}
.jp-icon-contrast1[fill] {
  fill: var(--jp-icon-contrast-color1);
}
.jp-icon-contrast2[fill] {
  fill: var(--jp-icon-contrast-color2);
}
.jp-icon-contrast3[fill] {
  fill: var(--jp-icon-contrast-color3);
}

.jp-icon-contrast0[stroke] {
  stroke: var(--jp-icon-contrast-color0);
}
.jp-icon-contrast1[stroke] {
  stroke: var(--jp-icon-contrast-color1);
}
.jp-icon-contrast2[stroke] {
  stroke: var(--jp-icon-contrast-color2);
}
.jp-icon-contrast3[stroke] {
  stroke: var(--jp-icon-contrast-color3);
}

/* CSS for icons in selected items in the settings editor */
#setting-editor .jp-PluginList .jp-mod-selected .jp-icon-selectable[fill] {
  fill: #fff;
}
#setting-editor
  .jp-PluginList
  .jp-mod-selected
  .jp-icon-selectable-inverse[fill] {
  fill: var(--jp-brand-color1);
}

/* CSS for icons in selected filebrowser listing items */
.jp-DirListing-item.jp-mod-selected .jp-icon-selectable[fill] {
  fill: #fff;
}
.jp-DirListing-item.jp-mod-selected .jp-icon-selectable-inverse[fill] {
  fill: var(--jp-brand-color1);
}

/* CSS for icons in selected tabs in the sidebar tab manager */
#tab-manager .lm-TabBar-tab.jp-mod-active .jp-icon-selectable[fill] {
  fill: #fff;
}

#tab-manager .lm-TabBar-tab.jp-mod-active .jp-icon-selectable-inverse[fill] {
  fill: var(--jp-brand-color1);
}
#tab-manager
  .lm-TabBar-tab.jp-mod-active
  .jp-icon-hover
  :hover
  .jp-icon-selectable[fill] {
  fill: var(--jp-brand-color1);
}

#tab-manager
  .lm-TabBar-tab.jp-mod-active
  .jp-icon-hover
  :hover
  .jp-icon-selectable-inverse[fill] {
  fill: #fff;
}

/**
 * TODO: come up with non css-hack solution for showing the busy icon on top
 *  of the close icon
 * CSS for complex behavior of close icon of tabs in the sidebar tab manager
 */
#tab-manager
  .lm-TabBar-tab.jp-mod-dirty
  > .lm-TabBar-tabCloseIcon
  > :not(:hover)
  > .jp-icon3[fill] {
  fill: none;
}
#tab-manager
  .lm-TabBar-tab.jp-mod-dirty
  > .lm-TabBar-tabCloseIcon
  > :not(:hover)
  > .jp-icon-busy[fill] {
  fill: var(--jp-inverse-layout-color3);
}

#tab-manager
  .lm-TabBar-tab.jp-mod-dirty.jp-mod-active
  > .lm-TabBar-tabCloseIcon
  > :not(:hover)
  > .jp-icon-busy[fill] {
  fill: #fff;
}

/**
* TODO: come up with non css-hack solution for showing the busy icon on top
*  of the close icon
* CSS for complex behavior of close icon of tabs in the main area tabbar
*/
.lm-DockPanel-tabBar
  .lm-TabBar-tab.lm-mod-closable.jp-mod-dirty
  > .lm-TabBar-tabCloseIcon
  > :not(:hover)
  > .jp-icon3[fill] {
  fill: none;
}
.lm-DockPanel-tabBar
  .lm-TabBar-tab.lm-mod-closable.jp-mod-dirty
  > .lm-TabBar-tabCloseIcon
  > :not(:hover)
  > .jp-icon-busy[fill] {
  fill: var(--jp-inverse-layout-color3);
}

/* CSS for icons in status bar */
#jp-main-statusbar .jp-mod-selected .jp-icon-selectable[fill] {
  fill: #fff;
}

#jp-main-statusbar .jp-mod-selected .jp-icon-selectable-inverse[fill] {
  fill: var(--jp-brand-color1);
}
/* special handling for splash icon CSS. While the theme CSS reloads during
   splash, the splash icon can loose theming. To prevent that, we set a
   default for its color variable */
:root {
  --jp-warn-color0: var(--md-orange-700);
}

/* not sure what to do with this one, used in filebrowser listing */
.jp-DragIcon {
  margin-right: 4px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/**
 * Support for alt colors for icons as inline SVG HTMLElements
 */

/* alt recolor the primary elements of an icon */
.jp-icon-alt .jp-icon0[fill] {
  fill: var(--jp-layout-color0);
}
.jp-icon-alt .jp-icon1[fill] {
  fill: var(--jp-layout-color1);
}
.jp-icon-alt .jp-icon2[fill] {
  fill: var(--jp-layout-color2);
}
.jp-icon-alt .jp-icon3[fill] {
  fill: var(--jp-layout-color3);
}
.jp-icon-alt .jp-icon4[fill] {
  fill: var(--jp-layout-color4);
}

.jp-icon-alt .jp-icon0[stroke] {
  stroke: var(--jp-layout-color0);
}
.jp-icon-alt .jp-icon1[stroke] {
  stroke: var(--jp-layout-color1);
}
.jp-icon-alt .jp-icon2[stroke] {
  stroke: var(--jp-layout-color2);
}
.jp-icon-alt .jp-icon3[stroke] {
  stroke: var(--jp-layout-color3);
}
.jp-icon-alt .jp-icon4[stroke] {
  stroke: var(--jp-layout-color4);
}

/* alt recolor the accent elements of an icon */
.jp-icon-alt .jp-icon-accent0[fill] {
  fill: var(--jp-inverse-layout-color0);
}
.jp-icon-alt .jp-icon-accent1[fill] {
  fill: var(--jp-inverse-layout-color1);
}
.jp-icon-alt .jp-icon-accent2[fill] {
  fill: var(--jp-inverse-layout-color2);
}
.jp-icon-alt .jp-icon-accent3[fill] {
  fill: var(--jp-inverse-layout-color3);
}
.jp-icon-alt .jp-icon-accent4[fill] {
  fill: var(--jp-inverse-layout-color4);
}

.jp-icon-alt .jp-icon-accent0[stroke] {
  stroke: var(--jp-inverse-layout-color0);
}
.jp-icon-alt .jp-icon-accent1[stroke] {
  stroke: var(--jp-inverse-layout-color1);
}
.jp-icon-alt .jp-icon-accent2[stroke] {
  stroke: var(--jp-inverse-layout-color2);
}
.jp-icon-alt .jp-icon-accent3[stroke] {
  stroke: var(--jp-inverse-layout-color3);
}
.jp-icon-alt .jp-icon-accent4[stroke] {
  stroke: var(--jp-inverse-layout-color4);
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-icon-hoverShow:not(:hover) svg {
  display: none !important;
}

/**
 * Support for hover colors for icons as inline SVG HTMLElements
 */

/**
 * regular colors
 */

/* recolor the primary elements of an icon */
.jp-icon-hover :hover .jp-icon0-hover[fill] {
  fill: var(--jp-inverse-layout-color0);
}
.jp-icon-hover :hover .jp-icon1-hover[fill] {
  fill: var(--jp-inverse-layout-color1);
}
.jp-icon-hover :hover .jp-icon2-hover[fill] {
  fill: var(--jp-inverse-layout-color2);
}
.jp-icon-hover :hover .jp-icon3-hover[fill] {
  fill: var(--jp-inverse-layout-color3);
}
.jp-icon-hover :hover .jp-icon4-hover[fill] {
  fill: var(--jp-inverse-layout-color4);
}

.jp-icon-hover :hover .jp-icon0-hover[stroke] {
  stroke: var(--jp-inverse-layout-color0);
}
.jp-icon-hover :hover .jp-icon1-hover[stroke] {
  stroke: var(--jp-inverse-layout-color1);
}
.jp-icon-hover :hover .jp-icon2-hover[stroke] {
  stroke: var(--jp-inverse-layout-color2);
}
.jp-icon-hover :hover .jp-icon3-hover[stroke] {
  stroke: var(--jp-inverse-layout-color3);
}
.jp-icon-hover :hover .jp-icon4-hover[stroke] {
  stroke: var(--jp-inverse-layout-color4);
}

/* recolor the accent elements of an icon */
.jp-icon-hover :hover .jp-icon-accent0-hover[fill] {
  fill: var(--jp-layout-color0);
}
.jp-icon-hover :hover .jp-icon-accent1-hover[fill] {
  fill: var(--jp-layout-color1);
}
.jp-icon-hover :hover .jp-icon-accent2-hover[fill] {
  fill: var(--jp-layout-color2);
}
.jp-icon-hover :hover .jp-icon-accent3-hover[fill] {
  fill: var(--jp-layout-color3);
}
.jp-icon-hover :hover .jp-icon-accent4-hover[fill] {
  fill: var(--jp-layout-color4);
}

.jp-icon-hover :hover .jp-icon-accent0-hover[stroke] {
  stroke: var(--jp-layout-color0);
}
.jp-icon-hover :hover .jp-icon-accent1-hover[stroke] {
  stroke: var(--jp-layout-color1);
}
.jp-icon-hover :hover .jp-icon-accent2-hover[stroke] {
  stroke: var(--jp-layout-color2);
}
.jp-icon-hover :hover .jp-icon-accent3-hover[stroke] {
  stroke: var(--jp-layout-color3);
}
.jp-icon-hover :hover .jp-icon-accent4-hover[stroke] {
  stroke: var(--jp-layout-color4);
}

/* set the color of an icon to transparent */
.jp-icon-hover :hover .jp-icon-none-hover[fill] {
  fill: none;
}

.jp-icon-hover :hover .jp-icon-none-hover[stroke] {
  stroke: none;
}

/**
 * inverse colors
 */

/* inverse recolor the primary elements of an icon */
.jp-icon-hover.jp-icon-alt :hover .jp-icon0-hover[fill] {
  fill: var(--jp-layout-color0);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon1-hover[fill] {
  fill: var(--jp-layout-color1);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon2-hover[fill] {
  fill: var(--jp-layout-color2);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon3-hover[fill] {
  fill: var(--jp-layout-color3);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon4-hover[fill] {
  fill: var(--jp-layout-color4);
}

.jp-icon-hover.jp-icon-alt :hover .jp-icon0-hover[stroke] {
  stroke: var(--jp-layout-color0);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon1-hover[stroke] {
  stroke: var(--jp-layout-color1);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon2-hover[stroke] {
  stroke: var(--jp-layout-color2);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon3-hover[stroke] {
  stroke: var(--jp-layout-color3);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon4-hover[stroke] {
  stroke: var(--jp-layout-color4);
}

/* inverse recolor the accent elements of an icon */
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent0-hover[fill] {
  fill: var(--jp-inverse-layout-color0);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent1-hover[fill] {
  fill: var(--jp-inverse-layout-color1);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent2-hover[fill] {
  fill: var(--jp-inverse-layout-color2);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent3-hover[fill] {
  fill: var(--jp-inverse-layout-color3);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent4-hover[fill] {
  fill: var(--jp-inverse-layout-color4);
}

.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent0-hover[stroke] {
  stroke: var(--jp-inverse-layout-color0);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent1-hover[stroke] {
  stroke: var(--jp-inverse-layout-color1);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent2-hover[stroke] {
  stroke: var(--jp-inverse-layout-color2);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent3-hover[stroke] {
  stroke: var(--jp-inverse-layout-color3);
}
.jp-icon-hover.jp-icon-alt :hover .jp-icon-accent4-hover[stroke] {
  stroke: var(--jp-inverse-layout-color4);
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-switch {
  display: flex;
  align-items: center;
  padding-left: 4px;
  padding-right: 4px;
  font-size: var(--jp-ui-font-size1);
  background-color: transparent;
  color: var(--jp-ui-font-color1);
  border: none;
  height: 20px;
}

.jp-switch:hover {
  background-color: var(--jp-layout-color2);
}

.jp-switch-label {
  margin-right: 5px;
}

.jp-switch-track {
  cursor: pointer;
  background-color: var(--jp-border-color1);
  -webkit-transition: 0.4s;
  transition: 0.4s;
  border-radius: 34px;
  height: 16px;
  width: 35px;
  position: relative;
}

.jp-switch-track::before {
  content: '';
  position: absolute;
  height: 10px;
  width: 10px;
  margin: 3px;
  left: 0px;
  background-color: var(--jp-ui-inverse-font-color1);
  -webkit-transition: 0.4s;
  transition: 0.4s;
  border-radius: 50%;
}

.jp-switch[aria-checked='true'] .jp-switch-track {
  background-color: var(--jp-warn-color0);
}

.jp-switch[aria-checked='true'] .jp-switch-track::before {
  /* track width (35) - margins (3 + 3) - thumb width (10) */
  left: 19px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/* Sibling imports */

/* Override Blueprint's _reset.scss styles */
html {
  box-sizing: unset;
}

*,
*::before,
*::after {
  box-sizing: unset;
}

body {
  color: unset;
  font-family: var(--jp-ui-font-family);
}

p {
  margin-top: unset;
  margin-bottom: unset;
}

small {
  font-size: unset;
}

strong {
  font-weight: unset;
}

/* Override Blueprint's _typography.scss styles */
a {
  text-decoration: unset;
  color: unset;
}
a:hover {
  text-decoration: unset;
  color: unset;
}

/* Override Blueprint's _accessibility.scss styles */
:focus {
  outline: unset;
  outline-offset: unset;
  -moz-outline-radius: unset;
}

/* Styles for ui-components */
.jp-Button {
  border-radius: var(--jp-border-radius);
  padding: 0px 12px;
  font-size: var(--jp-ui-font-size1);
}

/* Use our own theme for hover styles */
button.jp-Button.bp3-button.bp3-minimal:hover {
  background-color: var(--jp-layout-color2);
}
.jp-Button.minimal {
  color: unset !important;
}

.jp-Button.jp-ToolbarButtonComponent {
  text-transform: none;
}

.jp-InputGroup input {
  box-sizing: border-box;
  border-radius: 0;
  background-color: transparent;
  color: var(--jp-ui-font-color0);
  box-shadow: inset 0 0 0 var(--jp-border-width) var(--jp-input-border-color);
}

.jp-InputGroup input:focus {
  box-shadow: inset 0 0 0 var(--jp-border-width)
      var(--jp-input-active-box-shadow-color),
    inset 0 0 0 3px var(--jp-input-active-box-shadow-color);
}

.jp-InputGroup input::placeholder,
input::placeholder {
  color: var(--jp-ui-font-color3);
}

.jp-BPIcon {
  display: inline-block;
  vertical-align: middle;
  margin: auto;
}

/* Stop blueprint futzing with our icon fills */
.bp3-icon.jp-BPIcon > svg:not([fill]) {
  fill: var(--jp-inverse-layout-color3);
}

.jp-InputGroupAction {
  padding: 6px;
}

.jp-HTMLSelect.jp-DefaultStyle select {
  background-color: initial;
  border: none;
  border-radius: 0;
  box-shadow: none;
  color: var(--jp-ui-font-color0);
  display: block;
  font-size: var(--jp-ui-font-size1);
  height: 24px;
  line-height: 14px;
  padding: 0 25px 0 10px;
  text-align: left;
  -moz-appearance: none;
  -webkit-appearance: none;
}

/* Use our own theme for hover and option styles */
.jp-HTMLSelect.jp-DefaultStyle select:hover,
.jp-HTMLSelect.jp-DefaultStyle select > option {
  background-color: var(--jp-layout-color2);
  color: var(--jp-ui-font-color0);
}
select {
  box-sizing: border-box;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-Collapse {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  border-top: 1px solid var(--jp-border-color2);
  border-bottom: 1px solid var(--jp-border-color2);
}

.jp-Collapse-header {
  padding: 1px 12px;
  color: var(--jp-ui-font-color1);
  background-color: var(--jp-layout-color1);
  font-size: var(--jp-ui-font-size2);
}

.jp-Collapse-header:hover {
  background-color: var(--jp-layout-color2);
}

.jp-Collapse-contents {
  padding: 0px 12px 0px 12px;
  background-color: var(--jp-layout-color1);
  color: var(--jp-ui-font-color1);
  overflow: auto;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Variables
|----------------------------------------------------------------------------*/

:root {
  --jp-private-commandpalette-search-height: 28px;
}

/*-----------------------------------------------------------------------------
| Overall styles
|----------------------------------------------------------------------------*/

.lm-CommandPalette {
  padding-bottom: 0px;
  color: var(--jp-ui-font-color1);
  background: var(--jp-layout-color1);
  /* This is needed so that all font sizing of children done in ems is
   * relative to this base size */
  font-size: var(--jp-ui-font-size1);
}

/*-----------------------------------------------------------------------------
| Modal variant
|----------------------------------------------------------------------------*/

.jp-ModalCommandPalette {
  position: absolute;
  z-index: 10000;
  top: 38px;
  left: 30%;
  margin: 0;
  padding: 4px;
  width: 40%;
  box-shadow: var(--jp-elevation-z4);
  border-radius: 4px;
  background: var(--jp-layout-color0);
}

.jp-ModalCommandPalette .lm-CommandPalette {
  max-height: 40vh;
}

.jp-ModalCommandPalette .lm-CommandPalette .lm-close-icon::after {
  display: none;
}

.jp-ModalCommandPalette .lm-CommandPalette .lm-CommandPalette-header {
  display: none;
}

.jp-ModalCommandPalette .lm-CommandPalette .lm-CommandPalette-item {
  margin-left: 4px;
  margin-right: 4px;
}

.jp-ModalCommandPalette
  .lm-CommandPalette
  .lm-CommandPalette-item.lm-mod-disabled {
  display: none;
}

/*-----------------------------------------------------------------------------
| Search
|----------------------------------------------------------------------------*/

.lm-CommandPalette-search {
  padding: 4px;
  background-color: var(--jp-layout-color1);
  z-index: 2;
}

.lm-CommandPalette-wrapper {
  overflow: overlay;
  padding: 0px 9px;
  background-color: var(--jp-input-active-background);
  height: 30px;
  box-shadow: inset 0 0 0 var(--jp-border-width) var(--jp-input-border-color);
}

.lm-CommandPalette.lm-mod-focused .lm-CommandPalette-wrapper {
  box-shadow: inset 0 0 0 1px var(--jp-input-active-box-shadow-color),
    inset 0 0 0 3px var(--jp-input-active-box-shadow-color);
}

.jp-SearchIconGroup {
  color: white;
  background-color: var(--jp-brand-color1);
  position: absolute;
  top: 4px;
  right: 4px;
  padding: 5px 5px 1px 5px;
}

.jp-SearchIconGroup svg {
  height: 20px;
  width: 20px;
}

.jp-SearchIconGroup .jp-icon3[fill] {
  fill: var(--jp-layout-color0);
}

.lm-CommandPalette-input {
  background: transparent;
  width: calc(100% - 18px);
  float: left;
  border: none;
  outline: none;
  font-size: var(--jp-ui-font-size1);
  color: var(--jp-ui-font-color0);
  line-height: var(--jp-private-commandpalette-search-height);
}

.lm-CommandPalette-input::-webkit-input-placeholder,
.lm-CommandPalette-input::-moz-placeholder,
.lm-CommandPalette-input:-ms-input-placeholder {
  color: var(--jp-ui-font-color2);
  font-size: var(--jp-ui-font-size1);
}

/*-----------------------------------------------------------------------------
| Results
|----------------------------------------------------------------------------*/

.lm-CommandPalette-header:first-child {
  margin-top: 0px;
}

.lm-CommandPalette-header {
  border-bottom: solid var(--jp-border-width) var(--jp-border-color2);
  color: var(--jp-ui-font-color1);
  cursor: pointer;
  display: flex;
  font-size: var(--jp-ui-font-size0);
  font-weight: 600;
  letter-spacing: 1px;
  margin-top: 8px;
  padding: 8px 0 8px 12px;
  text-transform: uppercase;
}

.lm-CommandPalette-header.lm-mod-active {
  background: var(--jp-layout-color2);
}

.lm-CommandPalette-header > mark {
  background-color: transparent;
  font-weight: bold;
  color: var(--jp-ui-font-color1);
}

.lm-CommandPalette-item {
  padding: 4px 12px 4px 4px;
  color: var(--jp-ui-font-color1);
  font-size: var(--jp-ui-font-size1);
  font-weight: 400;
  display: flex;
}

.lm-CommandPalette-item.lm-mod-disabled {
  color: var(--jp-ui-font-color2);
}

.lm-CommandPalette-item.lm-mod-active {
  color: var(--jp-ui-inverse-font-color1);
  background: var(--jp-brand-color1);
}

.lm-CommandPalette-item.lm-mod-active .lm-CommandPalette-itemLabel > mark {
  color: var(--jp-ui-inverse-font-color0);
}

.lm-CommandPalette-item.lm-mod-active .jp-icon-selectable[fill] {
  fill: var(--jp-layout-color0);
}

.lm-CommandPalette-item.lm-mod-active .lm-CommandPalette-itemLabel > mark {
  color: var(--jp-ui-inverse-font-color0);
}

.lm-CommandPalette-item.lm-mod-active:hover:not(.lm-mod-disabled) {
  color: var(--jp-ui-inverse-font-color1);
  background: var(--jp-brand-color1);
}

.lm-CommandPalette-item:hover:not(.lm-mod-active):not(.lm-mod-disabled) {
  background: var(--jp-layout-color2);
}

.lm-CommandPalette-itemContent {
  overflow: hidden;
}

.lm-CommandPalette-itemLabel > mark {
  color: var(--jp-ui-font-color0);
  background-color: transparent;
  font-weight: bold;
}

.lm-CommandPalette-item.lm-mod-disabled mark {
  color: var(--jp-ui-font-color2);
}

.lm-CommandPalette-item .lm-CommandPalette-itemIcon {
  margin: 0 4px 0 0;
  position: relative;
  width: 16px;
  top: 2px;
  flex: 0 0 auto;
}

.lm-CommandPalette-item.lm-mod-disabled .lm-CommandPalette-itemIcon {
  opacity: 0.6;
}

.lm-CommandPalette-item .lm-CommandPalette-itemShortcut {
  flex: 0 0 auto;
}

.lm-CommandPalette-itemCaption {
  display: none;
}

.lm-CommandPalette-content {
  background-color: var(--jp-layout-color1);
}

.lm-CommandPalette-content:empty:after {
  content: 'No results';
  margin: auto;
  margin-top: 20px;
  width: 100px;
  display: block;
  font-size: var(--jp-ui-font-size2);
  font-family: var(--jp-ui-font-family);
  font-weight: lighter;
}

.lm-CommandPalette-emptyMessage {
  text-align: center;
  margin-top: 24px;
  line-height: 1.32;
  padding: 0px 8px;
  color: var(--jp-content-font-color3);
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-Dialog {
  position: absolute;
  z-index: 10000;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  top: 0px;
  left: 0px;
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  background: var(--jp-dialog-background);
}

.jp-Dialog-content {
  display: flex;
  flex-direction: column;
  margin-left: auto;
  margin-right: auto;
  background: var(--jp-layout-color1);
  padding: 24px;
  padding-bottom: 12px;
  min-width: 300px;
  min-height: 150px;
  max-width: 1000px;
  max-height: 500px;
  box-sizing: border-box;
  box-shadow: var(--jp-elevation-z20);
  word-wrap: break-word;
  border-radius: var(--jp-border-radius);
  /* This is needed so that all font sizing of children done in ems is
   * relative to this base size */
  font-size: var(--jp-ui-font-size1);
  color: var(--jp-ui-font-color1);
  resize: both;
}

.jp-Dialog-button {
  overflow: visible;
}

button.jp-Dialog-button:focus {
  outline: 1px solid var(--jp-brand-color1);
  outline-offset: 4px;
  -moz-outline-radius: 0px;
}

button.jp-Dialog-button:focus::-moz-focus-inner {
  border: 0;
}

button.jp-Dialog-close-button {
  padding: 0;
  height: 100%;
  min-width: unset;
  min-height: unset;
}

.jp-Dialog-header {
  display: flex;
  justify-content: space-between;
  flex: 0 0 auto;
  padding-bottom: 12px;
  font-size: var(--jp-ui-font-size3);
  font-weight: 400;
  color: var(--jp-ui-font-color0);
}

.jp-Dialog-body {
  display: flex;
  flex-direction: column;
  flex: 1 1 auto;
  font-size: var(--jp-ui-font-size1);
  background: var(--jp-layout-color1);
  overflow: auto;
}

.jp-Dialog-footer {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  flex: 0 0 auto;
  margin-left: -12px;
  margin-right: -12px;
  padding: 12px;
}

.jp-Dialog-title {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.jp-Dialog-body > .jp-select-wrapper {
  width: 100%;
}

.jp-Dialog-body > button {
  padding: 0px 16px;
}

.jp-Dialog-body > label {
  line-height: 1.4;
  color: var(--jp-ui-font-color0);
}

.jp-Dialog-button.jp-mod-styled:not(:last-child) {
  margin-right: 12px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2016, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-HoverBox {
  position: fixed;
}

.jp-HoverBox.jp-mod-outofview {
  display: none;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-IFrame {
  width: 100%;
  height: 100%;
}

.jp-IFrame > iframe {
  border: none;
}

/*
When drag events occur, `p-mod-override-cursor` is added to the body.
Because iframes steal all cursor events, the following two rules are necessary
to suppress pointer events while resize drags are occurring. There may be a
better solution to this problem.
*/
body.lm-mod-override-cursor .jp-IFrame {
  position: relative;
}

body.lm-mod-override-cursor .jp-IFrame:before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: transparent;
}

.jp-Input-Boolean-Dialog {
  flex-direction: row-reverse;
  align-items: end;
  width: 100%;
}

.jp-Input-Boolean-Dialog > label {
  flex: 1 1 auto;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2016, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-MainAreaWidget > :focus {
  outline: none;
}

/**
 * google-material-color v1.2.6
 * https://github.com/danlevan/google-material-color
 */
:root {
  --md-red-50: #ffebee;
  --md-red-100: #ffcdd2;
  --md-red-200: #ef9a9a;
  --md-red-300: #e57373;
  --md-red-400: #ef5350;
  --md-red-500: #f44336;
  --md-red-600: #e53935;
  --md-red-700: #d32f2f;
  --md-red-800: #c62828;
  --md-red-900: #b71c1c;
  --md-red-A100: #ff8a80;
  --md-red-A200: #ff5252;
  --md-red-A400: #ff1744;
  --md-red-A700: #d50000;

  --md-pink-50: #fce4ec;
  --md-pink-100: #f8bbd0;
  --md-pink-200: #f48fb1;
  --md-pink-300: #f06292;
  --md-pink-400: #ec407a;
  --md-pink-500: #e91e63;
  --md-pink-600: #d81b60;
  --md-pink-700: #c2185b;
  --md-pink-800: #ad1457;
  --md-pink-900: #880e4f;
  --md-pink-A100: #ff80ab;
  --md-pink-A200: #ff4081;
  --md-pink-A400: #f50057;
  --md-pink-A700: #c51162;

  --md-purple-50: #f3e5f5;
  --md-purple-100: #e1bee7;
  --md-purple-200: #ce93d8;
  --md-purple-300: #ba68c8;
  --md-purple-400: #ab47bc;
  --md-purple-500: #9c27b0;
  --md-purple-600: #8e24aa;
  --md-purple-700: #7b1fa2;
  --md-purple-800: #6a1b9a;
  --md-purple-900: #4a148c;
  --md-purple-A100: #ea80fc;
  --md-purple-A200: #e040fb;
  --md-purple-A400: #d500f9;
  --md-purple-A700: #aa00ff;

  --md-deep-purple-50: #ede7f6;
  --md-deep-purple-100: #d1c4e9;
  --md-deep-purple-200: #b39ddb;
  --md-deep-purple-300: #9575cd;
  --md-deep-purple-400: #7e57c2;
  --md-deep-purple-500: #673ab7;
  --md-deep-purple-600: #5e35b1;
  --md-deep-purple-700: #512da8;
  --md-deep-purple-800: #4527a0;
  --md-deep-purple-900: #311b92;
  --md-deep-purple-A100: #b388ff;
  --md-deep-purple-A200: #7c4dff;
  --md-deep-purple-A400: #651fff;
  --md-deep-purple-A700: #6200ea;

  --md-indigo-50: #e8eaf6;
  --md-indigo-100: #c5cae9;
  --md-indigo-200: #9fa8da;
  --md-indigo-300: #7986cb;
  --md-indigo-400: #5c6bc0;
  --md-indigo-500: #3f51b5;
  --md-indigo-600: #3949ab;
  --md-indigo-700: #303f9f;
  --md-indigo-800: #283593;
  --md-indigo-900: #1a237e;
  --md-indigo-A100: #8c9eff;
  --md-indigo-A200: #536dfe;
  --md-indigo-A400: #3d5afe;
  --md-indigo-A700: #304ffe;

  --md-blue-50: #e3f2fd;
  --md-blue-100: #bbdefb;
  --md-blue-200: #90caf9;
  --md-blue-300: #64b5f6;
  --md-blue-400: #42a5f5;
  --md-blue-500: #2196f3;
  --md-blue-600: #1e88e5;
  --md-blue-700: #1976d2;
  --md-blue-800: #1565c0;
  --md-blue-900: #0d47a1;
  --md-blue-A100: #82b1ff;
  --md-blue-A200: #448aff;
  --md-blue-A400: #2979ff;
  --md-blue-A700: #2962ff;

  --md-light-blue-50: #e1f5fe;
  --md-light-blue-100: #b3e5fc;
  --md-light-blue-200: #81d4fa;
  --md-light-blue-300: #4fc3f7;
  --md-light-blue-400: #29b6f6;
  --md-light-blue-500: #03a9f4;
  --md-light-blue-600: #039be5;
  --md-light-blue-700: #0288d1;
  --md-light-blue-800: #0277bd;
  --md-light-blue-900: #01579b;
  --md-light-blue-A100: #80d8ff;
  --md-light-blue-A200: #40c4ff;
  --md-light-blue-A400: #00b0ff;
  --md-light-blue-A700: #0091ea;

  --md-cyan-50: #e0f7fa;
  --md-cyan-100: #b2ebf2;
  --md-cyan-200: #80deea;
  --md-cyan-300: #4dd0e1;
  --md-cyan-400: #26c6da;
  --md-cyan-500: #00bcd4;
  --md-cyan-600: #00acc1;
  --md-cyan-700: #0097a7;
  --md-cyan-800: #00838f;
  --md-cyan-900: #006064;
  --md-cyan-A100: #84ffff;
  --md-cyan-A200: #18ffff;
  --md-cyan-A400: #00e5ff;
  --md-cyan-A700: #00b8d4;

  --md-teal-50: #e0f2f1;
  --md-teal-100: #b2dfdb;
  --md-teal-200: #80cbc4;
  --md-teal-300: #4db6ac;
  --md-teal-400: #26a69a;
  --md-teal-500: #009688;
  --md-teal-600: #00897b;
  --md-teal-700: #00796b;
  --md-teal-800: #00695c;
  --md-teal-900: #004d40;
  --md-teal-A100: #a7ffeb;
  --md-teal-A200: #64ffda;
  --md-teal-A400: #1de9b6;
  --md-teal-A700: #00bfa5;

  --md-green-50: #e8f5e9;
  --md-green-100: #c8e6c9;
  --md-green-200: #a5d6a7;
  --md-green-300: #81c784;
  --md-green-400: #66bb6a;
  --md-green-500: #4caf50;
  --md-green-600: #43a047;
  --md-green-700: #388e3c;
  --md-green-800: #2e7d32;
  --md-green-900: #1b5e20;
  --md-green-A100: #b9f6ca;
  --md-green-A200: #69f0ae;
  --md-green-A400: #00e676;
  --md-green-A700: #00c853;

  --md-light-green-50: #f1f8e9;
  --md-light-green-100: #dcedc8;
  --md-light-green-200: #c5e1a5;
  --md-light-green-300: #aed581;
  --md-light-green-400: #9ccc65;
  --md-light-green-500: #8bc34a;
  --md-light-green-600: #7cb342;
  --md-light-green-700: #689f38;
  --md-light-green-800: #558b2f;
  --md-light-green-900: #33691e;
  --md-light-green-A100: #ccff90;
  --md-light-green-A200: #b2ff59;
  --md-light-green-A400: #76ff03;
  --md-light-green-A700: #64dd17;

  --md-lime-50: #f9fbe7;
  --md-lime-100: #f0f4c3;
  --md-lime-200: #e6ee9c;
  --md-lime-300: #dce775;
  --md-lime-400: #d4e157;
  --md-lime-500: #cddc39;
  --md-lime-600: #c0ca33;
  --md-lime-700: #afb42b;
  --md-lime-800: #9e9d24;
  --md-lime-900: #827717;
  --md-lime-A100: #f4ff81;
  --md-lime-A200: #eeff41;
  --md-lime-A400: #c6ff00;
  --md-lime-A700: #aeea00;

  --md-yellow-50: #fffde7;
  --md-yellow-100: #fff9c4;
  --md-yellow-200: #fff59d;
  --md-yellow-300: #fff176;
  --md-yellow-400: #ffee58;
  --md-yellow-500: #ffeb3b;
  --md-yellow-600: #fdd835;
  --md-yellow-700: #fbc02d;
  --md-yellow-800: #f9a825;
  --md-yellow-900: #f57f17;
  --md-yellow-A100: #ffff8d;
  --md-yellow-A200: #ffff00;
  --md-yellow-A400: #ffea00;
  --md-yellow-A700: #ffd600;

  --md-amber-50: #fff8e1;
  --md-amber-100: #ffecb3;
  --md-amber-200: #ffe082;
  --md-amber-300: #ffd54f;
  --md-amber-400: #ffca28;
  --md-amber-500: #ffc107;
  --md-amber-600: #ffb300;
  --md-amber-700: #ffa000;
  --md-amber-800: #ff8f00;
  --md-amber-900: #ff6f00;
  --md-amber-A100: #ffe57f;
  --md-amber-A200: #ffd740;
  --md-amber-A400: #ffc400;
  --md-amber-A700: #ffab00;

  --md-orange-50: #fff3e0;
  --md-orange-100: #ffe0b2;
  --md-orange-200: #ffcc80;
  --md-orange-300: #ffb74d;
  --md-orange-400: #ffa726;
  --md-orange-500: #ff9800;
  --md-orange-600: #fb8c00;
  --md-orange-700: #f57c00;
  --md-orange-800: #ef6c00;
  --md-orange-900: #e65100;
  --md-orange-A100: #ffd180;
  --md-orange-A200: #ffab40;
  --md-orange-A400: #ff9100;
  --md-orange-A700: #ff6d00;

  --md-deep-orange-50: #fbe9e7;
  --md-deep-orange-100: #ffccbc;
  --md-deep-orange-200: #ffab91;
  --md-deep-orange-300: #ff8a65;
  --md-deep-orange-400: #ff7043;
  --md-deep-orange-500: #ff5722;
  --md-deep-orange-600: #f4511e;
  --md-deep-orange-700: #e64a19;
  --md-deep-orange-800: #d84315;
  --md-deep-orange-900: #bf360c;
  --md-deep-orange-A100: #ff9e80;
  --md-deep-orange-A200: #ff6e40;
  --md-deep-orange-A400: #ff3d00;
  --md-deep-orange-A700: #dd2c00;

  --md-brown-50: #efebe9;
  --md-brown-100: #d7ccc8;
  --md-brown-200: #bcaaa4;
  --md-brown-300: #a1887f;
  --md-brown-400: #8d6e63;
  --md-brown-500: #795548;
  --md-brown-600: #6d4c41;
  --md-brown-700: #5d4037;
  --md-brown-800: #4e342e;
  --md-brown-900: #3e2723;

  --md-grey-50: #fafafa;
  --md-grey-100: #f5f5f5;
  --md-grey-200: #eeeeee;
  --md-grey-300: #e0e0e0;
  --md-grey-400: #bdbdbd;
  --md-grey-500: #9e9e9e;
  --md-grey-600: #757575;
  --md-grey-700: #616161;
  --md-grey-800: #424242;
  --md-grey-900: #212121;

  --md-blue-grey-50: #eceff1;
  --md-blue-grey-100: #cfd8dc;
  --md-blue-grey-200: #b0bec5;
  --md-blue-grey-300: #90a4ae;
  --md-blue-grey-400: #78909c;
  --md-blue-grey-500: #607d8b;
  --md-blue-grey-600: #546e7a;
  --md-blue-grey-700: #455a64;
  --md-blue-grey-800: #37474f;
  --md-blue-grey-900: #263238;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2017, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-Spinner {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: var(--jp-layout-color0);
  outline: none;
}

.jp-SpinnerContent {
  font-size: 10px;
  margin: 50px auto;
  text-indent: -9999em;
  width: 3em;
  height: 3em;
  border-radius: 50%;
  background: var(--jp-brand-color3);
  background: linear-gradient(
    to right,
    #f37626 10%,
    rgba(255, 255, 255, 0) 42%
  );
  position: relative;
  animation: load3 1s infinite linear, fadeIn 1s;
}

.jp-SpinnerContent:before {
  width: 50%;
  height: 50%;
  background: #f37626;
  border-radius: 100% 0 0 0;
  position: absolute;
  top: 0;
  left: 0;
  content: '';
}

.jp-SpinnerContent:after {
  background: var(--jp-layout-color0);
  width: 75%;
  height: 75%;
  border-radius: 50%;
  content: '';
  margin: auto;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
}

@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@keyframes load3 {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

button.jp-mod-styled {
  font-size: var(--jp-ui-font-size1);
  color: var(--jp-ui-font-color0);
  border: none;
  box-sizing: border-box;
  text-align: center;
  line-height: 32px;
  height: 32px;
  padding: 0px 12px;
  letter-spacing: 0.8px;
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
}

input.jp-mod-styled {
  background: var(--jp-input-background);
  height: 28px;
  box-sizing: border-box;
  border: var(--jp-border-width) solid var(--jp-border-color1);
  padding-left: 7px;
  padding-right: 7px;
  font-size: var(--jp-ui-font-size2);
  color: var(--jp-ui-font-color0);
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
}

input[type='checkbox'].jp-mod-styled {
  appearance: checkbox;
  -webkit-appearance: checkbox;
  -moz-appearance: checkbox;
  height: auto;
}

input.jp-mod-styled:focus {
  border: var(--jp-border-width) solid var(--md-blue-500);
  box-shadow: inset 0 0 4px var(--md-blue-300);
}

.jp-FileDialog-Checkbox {
  margin-top: 35px;
  display: flex;
  flex-direction: row;
  align-items: end;
  width: 100%;
}

.jp-FileDialog-Checkbox > label {
  flex: 1 1 auto;
}

.jp-select-wrapper {
  display: flex;
  position: relative;
  flex-direction: column;
  padding: 1px;
  background-color: var(--jp-layout-color1);
  height: 28px;
  box-sizing: border-box;
  margin-bottom: 12px;
}

.jp-select-wrapper.jp-mod-focused select.jp-mod-styled {
  border: var(--jp-border-width) solid var(--jp-input-active-border-color);
  box-shadow: var(--jp-input-box-shadow);
  background-color: var(--jp-input-active-background);
}

select.jp-mod-styled:hover {
  background-color: var(--jp-layout-color1);
  cursor: pointer;
  color: var(--jp-ui-font-color0);
  background-color: var(--jp-input-hover-background);
  box-shadow: inset 0 0px 1px rgba(0, 0, 0, 0.5);
}

select.jp-mod-styled {
  flex: 1 1 auto;
  height: 32px;
  width: 100%;
  font-size: var(--jp-ui-font-size2);
  background: var(--jp-input-background);
  color: var(--jp-ui-font-color0);
  padding: 0 25px 0 8px;
  border: var(--jp-border-width) solid var(--jp-input-border-color);
  border-radius: 0px;
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2016, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

:root {
  --jp-private-toolbar-height: calc(
    28px + var(--jp-border-width)
  ); /* leave 28px for content */
}

.jp-Toolbar {
  color: var(--jp-ui-font-color1);
  flex: 0 0 auto;
  display: flex;
  flex-direction: row;
  border-bottom: var(--jp-border-width) solid var(--jp-toolbar-border-color);
  box-shadow: var(--jp-toolbar-box-shadow);
  background: var(--jp-toolbar-background);
  min-height: var(--jp-toolbar-micro-height);
  padding: 2px;
  z-index: 1;
  overflow-x: auto;
}

/* Toolbar items */

.jp-Toolbar > .jp-Toolbar-item.jp-Toolbar-spacer {
  flex-grow: 1;
  flex-shrink: 1;
}

.jp-Toolbar-item.jp-Toolbar-kernelStatus {
  display: inline-block;
  width: 32px;
  background-repeat: no-repeat;
  background-position: center;
  background-size: 16px;
}

.jp-Toolbar > .jp-Toolbar-item {
  flex: 0 0 auto;
  display: flex;
  padding-left: 1px;
  padding-right: 1px;
  font-size: var(--jp-ui-font-size1);
  line-height: var(--jp-private-toolbar-height);
  height: 100%;
}

/* Toolbar buttons */

/* This is the div we use to wrap the react component into a Widget */
div.jp-ToolbarButton {
  color: transparent;
  border: none;
  box-sizing: border-box;
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  padding: 0px;
  margin: 0px;
}

button.jp-ToolbarButtonComponent {
  background: var(--jp-layout-color1);
  border: none;
  box-sizing: border-box;
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  padding: 0px 6px;
  margin: 0px;
  height: 24px;
  border-radius: var(--jp-border-radius);
  display: flex;
  align-items: center;
  text-align: center;
  font-size: 14px;
  min-width: unset;
  min-height: unset;
}

button.jp-ToolbarButtonComponent:disabled {
  opacity: 0.4;
}

button.jp-ToolbarButtonComponent span {
  padding: 0px;
  flex: 0 0 auto;
}

button.jp-ToolbarButtonComponent .jp-ToolbarButtonComponent-label {
  font-size: var(--jp-ui-font-size1);
  line-height: 100%;
  padding-left: 2px;
  color: var(--jp-ui-font-color1);
}

#jp-main-dock-panel[data-mode='single-document']
  .jp-MainAreaWidget
  > .jp-Toolbar.jp-Toolbar-micro {
  padding: 0;
  min-height: 0;
}

#jp-main-dock-panel[data-mode='single-document']
  .jp-MainAreaWidget
  > .jp-Toolbar {
  border: none;
  box-shadow: none;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2017, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Copyright (c) 2014-2017, PhosphorJS Contributors
|
| Distributed under the terms of the BSD 3-Clause License.
|
| The full license is in the file LICENSE, distributed with this software.
|----------------------------------------------------------------------------*/


/* <DEPRECATED> */ body.p-mod-override-cursor *, /*  */
body.lm-mod-override-cursor * {
  cursor: inherit !important;
}

/*-----------------------------------------------------------------------------
| Copyright (c) 2014-2016, Jupyter Development Team.
|
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-JSONEditor {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.jp-JSONEditor-host {
  flex: 1 1 auto;
  border: var(--jp-border-width) solid var(--jp-input-border-color);
  border-radius: 0px;
  background: var(--jp-layout-color0);
  min-height: 50px;
  padding: 1px;
}

.jp-JSONEditor.jp-mod-error .jp-JSONEditor-host {
  border-color: red;
  outline-color: red;
}

.jp-JSONEditor-header {
  display: flex;
  flex: 1 0 auto;
  padding: 0 0 0 12px;
}

.jp-JSONEditor-header label {
  flex: 0 0 auto;
}

.jp-JSONEditor-commitButton {
  height: 16px;
  width: 16px;
  background-size: 18px;
  background-repeat: no-repeat;
  background-position: center;
}

.jp-JSONEditor-host.jp-mod-focused {
  background-color: var(--jp-input-active-background);
  border: 1px solid var(--jp-input-active-border-color);
  box-shadow: var(--jp-input-box-shadow);
}

.jp-Editor.jp-mod-dropTarget {
  border: var(--jp-border-width) solid var(--jp-input-active-border-color);
  box-shadow: var(--jp-input-box-shadow);
}

/* BASICS */

.CodeMirror {
  /* Set height, width, borders, and global font properties here */
  font-family: monospace;
  height: 300px;
  color: black;
  direction: ltr;
}

/* PADDING */

.CodeMirror-lines {
  padding: 4px 0; /* Vertical padding around content */
}
.CodeMirror pre.CodeMirror-line,
.CodeMirror pre.CodeMirror-line-like {
  padding: 0 4px; /* Horizontal padding of content */
}

.CodeMirror-scrollbar-filler, .CodeMirror-gutter-filler {
  background-color: white; /* The little square between H and V scrollbars */
}

/* GUTTER */

.CodeMirror-gutters {
  border-right: 1px solid #ddd;
  background-color: #f7f7f7;
  white-space: nowrap;
}
.CodeMirror-linenumbers {}
.CodeMirror-linenumber {
  padding: 0 3px 0 5px;
  min-width: 20px;
  text-align: right;
  color: #999;
  white-space: nowrap;
}

.CodeMirror-guttermarker { color: black; }
.CodeMirror-guttermarker-subtle { color: #999; }

/* CURSOR */

.CodeMirror-cursor {
  border-left: 1px solid black;
  border-right: none;
  width: 0;
}
/* Shown when moving in bi-directional text */
.CodeMirror div.CodeMirror-secondarycursor {
  border-left: 1px solid silver;
}
.cm-fat-cursor .CodeMirror-cursor {
  width: auto;
  border: 0 !important;
  background: #7e7;
}
.cm-fat-cursor div.CodeMirror-cursors {
  z-index: 1;
}
.cm-fat-cursor-mark {
  background-color: rgba(20, 255, 20, 0.5);
  -webkit-animation: blink 1.06s steps(1) infinite;
  -moz-animation: blink 1.06s steps(1) infinite;
  animation: blink 1.06s steps(1) infinite;
}
.cm-animate-fat-cursor {
  width: auto;
  border: 0;
  -webkit-animation: blink 1.06s steps(1) infinite;
  -moz-animation: blink 1.06s steps(1) infinite;
  animation: blink 1.06s steps(1) infinite;
  background-color: #7e7;
}
@-moz-keyframes blink {
  0% {}
  50% { background-color: transparent; }
  100% {}
}
@-webkit-keyframes blink {
  0% {}
  50% { background-color: transparent; }
  100% {}
}
@keyframes blink {
  0% {}
  50% { background-color: transparent; }
  100% {}
}

/* Can style cursor different in overwrite (non-insert) mode */
.CodeMirror-overwrite .CodeMirror-cursor {}

.cm-tab { display: inline-block; text-decoration: inherit; }

.CodeMirror-rulers {
  position: absolute;
  left: 0; right: 0; top: -50px; bottom: 0;
  overflow: hidden;
}
.CodeMirror-ruler {
  border-left: 1px solid #ccc;
  top: 0; bottom: 0;
  position: absolute;
}

/* DEFAULT THEME */

.cm-s-default .cm-header {color: blue;}
.cm-s-default .cm-quote {color: #090;}
.cm-negative {color: #d44;}
.cm-positive {color: #292;}
.cm-header, .cm-strong {font-weight: bold;}
.cm-em {font-style: italic;}
.cm-link {text-decoration: underline;}
.cm-strikethrough {text-decoration: line-through;}

.cm-s-default .cm-keyword {color: #708;}
.cm-s-default .cm-atom {color: #219;}
.cm-s-default .cm-number {color: #164;}
.cm-s-default .cm-def {color: #00f;}
.cm-s-default .cm-variable,
.cm-s-default .cm-punctuation,
.cm-s-default .cm-property,
.cm-s-default .cm-operator {}
.cm-s-default .cm-variable-2 {color: #05a;}
.cm-s-default .cm-variable-3, .cm-s-default .cm-type {color: #085;}
.cm-s-default .cm-comment {color: #a50;}
.cm-s-default .cm-string {color: #a11;}
.cm-s-default .cm-string-2 {color: #f50;}
.cm-s-default .cm-meta {color: #555;}
.cm-s-default .cm-qualifier {color: #555;}
.cm-s-default .cm-builtin {color: #30a;}
.cm-s-default .cm-bracket {color: #997;}
.cm-s-default .cm-tag {color: #170;}
.cm-s-default .cm-attribute {color: #00c;}
.cm-s-default .cm-hr {color: #999;}
.cm-s-default .cm-link {color: #00c;}

.cm-s-default .cm-error {color: #f00;}
.cm-invalidchar {color: #f00;}

.CodeMirror-composing { border-bottom: 2px solid; }

/* Default styles for common addons */

div.CodeMirror span.CodeMirror-matchingbracket {color: #0b0;}
div.CodeMirror span.CodeMirror-nonmatchingbracket {color: #a22;}
.CodeMirror-matchingtag { background: rgba(255, 150, 0, .3); }
.CodeMirror-activeline-background {background: #e8f2ff;}

/* STOP */

/* The rest of this file contains styles related to the mechanics of
   the editor. You probably shouldn't touch them. */

.CodeMirror {
  position: relative;
  overflow: hidden;
  background: white;
}

.CodeMirror-scroll {
  overflow: scroll !important; /* Things will break if this is overridden */
  /* 50px is the magic margin used to hide the element's real scrollbars */
  /* See overflow: hidden in .CodeMirror */
  margin-bottom: -50px; margin-right: -50px;
  padding-bottom: 50px;
  height: 100%;
  outline: none; /* Prevent dragging from highlighting the element */
  position: relative;
}
.CodeMirror-sizer {
  position: relative;
  border-right: 50px solid transparent;
}

/* The fake, visible scrollbars. Used to force redraw during scrolling
   before actual scrolling happens, thus preventing shaking and
   flickering artifacts. */
.CodeMirror-vscrollbar, .CodeMirror-hscrollbar, .CodeMirror-scrollbar-filler, .CodeMirror-gutter-filler {
  position: absolute;
  z-index: 6;
  display: none;
  outline: none;
}
.CodeMirror-vscrollbar {
  right: 0; top: 0;
  overflow-x: hidden;
  overflow-y: scroll;
}
.CodeMirror-hscrollbar {
  bottom: 0; left: 0;
  overflow-y: hidden;
  overflow-x: scroll;
}
.CodeMirror-scrollbar-filler {
  right: 0; bottom: 0;
}
.CodeMirror-gutter-filler {
  left: 0; bottom: 0;
}

.CodeMirror-gutters {
  position: absolute; left: 0; top: 0;
  min-height: 100%;
  z-index: 3;
}
.CodeMirror-gutter {
  white-space: normal;
  height: 100%;
  display: inline-block;
  vertical-align: top;
  margin-bottom: -50px;
}
.CodeMirror-gutter-wrapper {
  position: absolute;
  z-index: 4;
  background: none !important;
  border: none !important;
}
.CodeMirror-gutter-background {
  position: absolute;
  top: 0; bottom: 0;
  z-index: 4;
}
.CodeMirror-gutter-elt {
  position: absolute;
  cursor: default;
  z-index: 4;
}
.CodeMirror-gutter-wrapper ::selection { background-color: transparent }
.CodeMirror-gutter-wrapper ::-moz-selection { background-color: transparent }

.CodeMirror-lines {
  cursor: text;
  min-height: 1px; /* prevents collapsing before first draw */
}
.CodeMirror pre.CodeMirror-line,
.CodeMirror pre.CodeMirror-line-like {
  /* Reset some styles that the rest of the page might have set */
  -moz-border-radius: 0; -webkit-border-radius: 0; border-radius: 0;
  border-width: 0;
  background: transparent;
  font-family: inherit;
  font-size: inherit;
  margin: 0;
  white-space: pre;
  word-wrap: normal;
  line-height: inherit;
  color: inherit;
  z-index: 2;
  position: relative;
  overflow: visible;
  -webkit-tap-highlight-color: transparent;
  -webkit-font-variant-ligatures: contextual;
  font-variant-ligatures: contextual;
}
.CodeMirror-wrap pre.CodeMirror-line,
.CodeMirror-wrap pre.CodeMirror-line-like {
  word-wrap: break-word;
  white-space: pre-wrap;
  word-break: normal;
}

.CodeMirror-linebackground {
  position: absolute;
  left: 0; right: 0; top: 0; bottom: 0;
  z-index: 0;
}

.CodeMirror-linewidget {
  position: relative;
  z-index: 2;
  padding: 0.1px; /* Force widget margins to stay inside of the container */
}

.CodeMirror-widget {}

.CodeMirror-rtl pre { direction: rtl; }

.CodeMirror-code {
  outline: none;
}

/* Force content-box sizing for the elements where we expect it */
.CodeMirror-scroll,
.CodeMirror-sizer,
.CodeMirror-gutter,
.CodeMirror-gutters,
.CodeMirror-linenumber {
  -moz-box-sizing: content-box;
  box-sizing: content-box;
}

.CodeMirror-measure {
  position: absolute;
  width: 100%;
  height: 0;
  overflow: hidden;
  visibility: hidden;
}

.CodeMirror-cursor {
  position: absolute;
  pointer-events: none;
}
.CodeMirror-measure pre { position: static; }

div.CodeMirror-cursors {
  visibility: hidden;
  position: relative;
  z-index: 3;
}
div.CodeMirror-dragcursors {
  visibility: visible;
}

.CodeMirror-focused div.CodeMirror-cursors {
  visibility: visible;
}

.CodeMirror-selected { background: #d9d9d9; }
.CodeMirror-focused .CodeMirror-selected { background: #d7d4f0; }
.CodeMirror-crosshair { cursor: crosshair; }
.CodeMirror-line::selection, .CodeMirror-line > span::selection, .CodeMirror-line > span > span::selection { background: #d7d4f0; }
.CodeMirror-line::-moz-selection, .CodeMirror-line > span::-moz-selection, .CodeMirror-line > span > span::-moz-selection { background: #d7d4f0; }

.cm-searching {
  background-color: #ffa;
  background-color: rgba(255, 255, 0, .4);
}

/* Used to force a border model for a node */
.cm-force-border { padding-right: .1px; }

@media print {
  /* Hide the cursor when printing */
  .CodeMirror div.CodeMirror-cursors {
    visibility: hidden;
  }
}

/* See issue #2901 */
.cm-tab-wrap-hack:after { content: ''; }

/* Help users use markselection to safely style text background */
span.CodeMirror-selectedtext { background: none; }

.CodeMirror-dialog {
  position: absolute;
  left: 0; right: 0;
  background: inherit;
  z-index: 15;
  padding: .1em .8em;
  overflow: hidden;
  color: inherit;
}

.CodeMirror-dialog-top {
  border-bottom: 1px solid #eee;
  top: 0;
}

.CodeMirror-dialog-bottom {
  border-top: 1px solid #eee;
  bottom: 0;
}

.CodeMirror-dialog input {
  border: none;
  outline: none;
  background: transparent;
  width: 20em;
  color: inherit;
  font-family: monospace;
}

.CodeMirror-dialog button {
  font-size: 70%;
}

.CodeMirror-foldmarker {
  color: blue;
  text-shadow: #b9f 1px 1px 2px, #b9f -1px -1px 2px, #b9f 1px -1px 2px, #b9f -1px 1px 2px;
  font-family: arial;
  line-height: .3;
  cursor: pointer;
}
.CodeMirror-foldgutter {
  width: .7em;
}
.CodeMirror-foldgutter-open,
.CodeMirror-foldgutter-folded {
  cursor: pointer;
}
.CodeMirror-foldgutter-open:after {
  content: "\25BE";
}
.CodeMirror-foldgutter-folded:after {
  content: "\25B8";
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.CodeMirror {
  line-height: var(--jp-code-line-height);
  font-size: var(--jp-code-font-size);
  font-family: var(--jp-code-font-family);
  border: 0;
  border-radius: 0;
  height: auto;
  /* Changed to auto to autogrow */
}

.CodeMirror pre {
  padding: 0 var(--jp-code-padding);
}

.jp-CodeMirrorEditor[data-type='inline'] .CodeMirror-dialog {
  background-color: var(--jp-layout-color0);
  color: var(--jp-content-font-color1);
}

/* This causes https://github.com/jupyter/jupyterlab/issues/522 */
/* May not cause it not because we changed it! */
.CodeMirror-lines {
  padding: var(--jp-code-padding) 0;
}

.CodeMirror-linenumber {
  padding: 0 8px;
}

.jp-CodeMirrorEditor {
  cursor: text;
}

.jp-CodeMirrorEditor[data-type='inline'] .CodeMirror-cursor {
  border-left: var(--jp-code-cursor-width0) solid var(--jp-editor-cursor-color);
}

/* When zoomed out 67% and 33% on a screen of 1440 width x 900 height */
@media screen and (min-width: 2138px) and (max-width: 4319px) {
  .jp-CodeMirrorEditor[data-type='inline'] .CodeMirror-cursor {
    border-left: var(--jp-code-cursor-width1) solid
      var(--jp-editor-cursor-color);
  }
}

/* When zoomed out less than 33% */
@media screen and (min-width: 4320px) {
  .jp-CodeMirrorEditor[data-type='inline'] .CodeMirror-cursor {
    border-left: var(--jp-code-cursor-width2) solid
      var(--jp-editor-cursor-color);
  }
}

.CodeMirror.jp-mod-readOnly .CodeMirror-cursor {
  display: none;
}

.CodeMirror-gutters {
  border-right: 1px solid var(--jp-border-color2);
  background-color: var(--jp-layout-color0);
}

.jp-CollaboratorCursor {
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  border-top: none;
  border-bottom: 3px solid;
  background-clip: content-box;
  margin-left: -5px;
  margin-right: -5px;
}

.CodeMirror-selectedtext.cm-searching {
  background-color: var(--jp-search-selected-match-background-color) !important;
  color: var(--jp-search-selected-match-color) !important;
}

.cm-searching {
  background-color: var(
    --jp-search-unselected-match-background-color
  ) !important;
  color: var(--jp-search-unselected-match-color) !important;
}

.CodeMirror-focused .CodeMirror-selected {
  background-color: var(--jp-editor-selected-focused-background);
}

.CodeMirror-selected {
  background-color: var(--jp-editor-selected-background);
}

.jp-CollaboratorCursor-hover {
  position: absolute;
  z-index: 1;
  transform: translateX(-50%);
  color: white;
  border-radius: 3px;
  padding-left: 4px;
  padding-right: 4px;
  padding-top: 1px;
  padding-bottom: 1px;
  text-align: center;
  font-size: var(--jp-ui-font-size1);
  white-space: nowrap;
}

.jp-CodeMirror-ruler {
  border-left: 1px dashed var(--jp-border-color2);
}

/**
 * Here is our jupyter theme for CodeMirror syntax highlighting
 * This is used in our marked.js syntax highlighting and CodeMirror itself
 * The string "jupyter" is set in ../codemirror/widget.DEFAULT_CODEMIRROR_THEME
 * This came from the classic notebook, which came form highlight.js/GitHub
 */

/**
 * CodeMirror themes are handling the background/color in this way. This works
 * fine for CodeMirror editors outside the notebook, but the notebook styles
 * these things differently.
 */
.CodeMirror.cm-s-jupyter {
  background: var(--jp-layout-color0);
  color: var(--jp-content-font-color1);
}

/* In the notebook, we want this styling to be handled by its container */
.jp-CodeConsole .CodeMirror.cm-s-jupyter,
.jp-Notebook .CodeMirror.cm-s-jupyter {
  background: transparent;
}

.cm-s-jupyter .CodeMirror-cursor {
  border-left: var(--jp-code-cursor-width0) solid var(--jp-editor-cursor-color);
}
.cm-s-jupyter span.cm-keyword {
  color: var(--jp-mirror-editor-keyword-color);
  font-weight: bold;
}
.cm-s-jupyter span.cm-atom {
  color: var(--jp-mirror-editor-atom-color);
}
.cm-s-jupyter span.cm-number {
  color: var(--jp-mirror-editor-number-color);
}
.cm-s-jupyter span.cm-def {
  color: var(--jp-mirror-editor-def-color);
}
.cm-s-jupyter span.cm-variable {
  color: var(--jp-mirror-editor-variable-color);
}
.cm-s-jupyter span.cm-variable-2 {
  color: var(--jp-mirror-editor-variable-2-color);
}
.cm-s-jupyter span.cm-variable-3 {
  color: var(--jp-mirror-editor-variable-3-color);
}
.cm-s-jupyter span.cm-punctuation {
  color: var(--jp-mirror-editor-punctuation-color);
}
.cm-s-jupyter span.cm-property {
  color: var(--jp-mirror-editor-property-color);
}
.cm-s-jupyter span.cm-operator {
  color: var(--jp-mirror-editor-operator-color);
  font-weight: bold;
}
.cm-s-jupyter span.cm-comment {
  color: var(--jp-mirror-editor-comment-color);
  font-style: italic;
}
.cm-s-jupyter span.cm-string {
  color: var(--jp-mirror-editor-string-color);
}
.cm-s-jupyter span.cm-string-2 {
  color: var(--jp-mirror-editor-string-2-color);
}
.cm-s-jupyter span.cm-meta {
  color: var(--jp-mirror-editor-meta-color);
}
.cm-s-jupyter span.cm-qualifier {
  color: var(--jp-mirror-editor-qualifier-color);
}
.cm-s-jupyter span.cm-builtin {
  color: var(--jp-mirror-editor-builtin-color);
}
.cm-s-jupyter span.cm-bracket {
  color: var(--jp-mirror-editor-bracket-color);
}
.cm-s-jupyter span.cm-tag {
  color: var(--jp-mirror-editor-tag-color);
}
.cm-s-jupyter span.cm-attribute {
  color: var(--jp-mirror-editor-attribute-color);
}
.cm-s-jupyter span.cm-header {
  color: var(--jp-mirror-editor-header-color);
}
.cm-s-jupyter span.cm-quote {
  color: var(--jp-mirror-editor-quote-color);
}
.cm-s-jupyter span.cm-link {
  color: var(--jp-mirror-editor-link-color);
}
.cm-s-jupyter span.cm-error {
  color: var(--jp-mirror-editor-error-color);
}
.cm-s-jupyter span.cm-hr {
  color: #999;
}

.cm-s-jupyter span.cm-tab {
  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAMCAYAAAAkuj5RAAAAAXNSR0IArs4c6QAAAGFJREFUSMft1LsRQFAQheHPowAKoACx3IgEKtaEHujDjORSgWTH/ZOdnZOcM/sgk/kFFWY0qV8foQwS4MKBCS3qR6ixBJvElOobYAtivseIE120FaowJPN75GMu8j/LfMwNjh4HUpwg4LUAAAAASUVORK5CYII=);
  background-position: right;
  background-repeat: no-repeat;
}

.cm-s-jupyter .CodeMirror-activeline-background,
.cm-s-jupyter .CodeMirror-gutter {
  background-color: var(--jp-layout-color2);
}

/* Styles for shared cursors (remote cursor locations and selected ranges) */
.jp-CodeMirrorEditor .remote-caret {
  position: relative;
  border-left: 2px solid black;
  margin-left: -1px;
  margin-right: -1px;
  box-sizing: border-box;
}

.jp-CodeMirrorEditor .remote-caret > div {
  white-space: nowrap;
  position: absolute;
  top: -1.15em;
  padding-bottom: 0.05em;
  left: -2px;
  font-size: 0.95em;
  background-color: rgb(250, 129, 0);
  font-family: var(--jp-ui-font-family);
  font-weight: bold;
  line-height: normal;
  user-select: none;
  color: white;
  padding-left: 2px;
  padding-right: 2px;
  z-index: 3;
  transition: opacity 0.3s ease-in-out;
}

.jp-CodeMirrorEditor .remote-caret.hide-name > div {
  transition-delay: 0.7s;
  opacity: 0;
}

.jp-CodeMirrorEditor .remote-caret:hover > div {
  opacity: 1;
  transition-delay: 0s;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| RenderedText
|----------------------------------------------------------------------------*/

:root {
  /* This is the padding value to fill the gaps between lines containing spans with background color. */
  --jp-private-code-span-padding: calc(
    (var(--jp-code-line-height) - 1) * var(--jp-code-font-size) / 2
  );
}

.jp-RenderedText {
  text-align: left;
  padding-left: var(--jp-code-padding);
  line-height: var(--jp-code-line-height);
  font-family: var(--jp-code-font-family);
}

.jp-RenderedText pre,
.jp-RenderedJavaScript pre,
.jp-RenderedHTMLCommon pre {
  color: var(--jp-content-font-color1);
  font-size: var(--jp-code-font-size);
  border: none;
  margin: 0px;
  padding: 0px;
}

.jp-RenderedText pre a:link {
  text-decoration: none;
  color: var(--jp-content-link-color);
}
.jp-RenderedText pre a:hover {
  text-decoration: underline;
  color: var(--jp-content-link-color);
}
.jp-RenderedText pre a:visited {
  text-decoration: none;
  color: var(--jp-content-link-color);
}

/* console foregrounds and backgrounds */
.jp-RenderedText pre .ansi-black-fg {
  color: #3e424d;
}
.jp-RenderedText pre .ansi-red-fg {
  color: #e75c58;
}
.jp-RenderedText pre .ansi-green-fg {
  color: #00a250;
}
.jp-RenderedText pre .ansi-yellow-fg {
  color: #ddb62b;
}
.jp-RenderedText pre .ansi-blue-fg {
  color: #208ffb;
}
.jp-RenderedText pre .ansi-magenta-fg {
  color: #d160c4;
}
.jp-RenderedText pre .ansi-cyan-fg {
  color: #60c6c8;
}
.jp-RenderedText pre .ansi-white-fg {
  color: #c5c1b4;
}

.jp-RenderedText pre .ansi-black-bg {
  background-color: #3e424d;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-red-bg {
  background-color: #e75c58;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-green-bg {
  background-color: #00a250;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-yellow-bg {
  background-color: #ddb62b;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-blue-bg {
  background-color: #208ffb;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-magenta-bg {
  background-color: #d160c4;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-cyan-bg {
  background-color: #60c6c8;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-white-bg {
  background-color: #c5c1b4;
  padding: var(--jp-private-code-span-padding) 0;
}

.jp-RenderedText pre .ansi-black-intense-fg {
  color: #282c36;
}
.jp-RenderedText pre .ansi-red-intense-fg {
  color: #b22b31;
}
.jp-RenderedText pre .ansi-green-intense-fg {
  color: #007427;
}
.jp-RenderedText pre .ansi-yellow-intense-fg {
  color: #b27d12;
}
.jp-RenderedText pre .ansi-blue-intense-fg {
  color: #0065ca;
}
.jp-RenderedText pre .ansi-magenta-intense-fg {
  color: #a03196;
}
.jp-RenderedText pre .ansi-cyan-intense-fg {
  color: #258f8f;
}
.jp-RenderedText pre .ansi-white-intense-fg {
  color: #a1a6b2;
}

.jp-RenderedText pre .ansi-black-intense-bg {
  background-color: #282c36;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-red-intense-bg {
  background-color: #b22b31;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-green-intense-bg {
  background-color: #007427;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-yellow-intense-bg {
  background-color: #b27d12;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-blue-intense-bg {
  background-color: #0065ca;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-magenta-intense-bg {
  background-color: #a03196;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-cyan-intense-bg {
  background-color: #258f8f;
  padding: var(--jp-private-code-span-padding) 0;
}
.jp-RenderedText pre .ansi-white-intense-bg {
  background-color: #a1a6b2;
  padding: var(--jp-private-code-span-padding) 0;
}

.jp-RenderedText pre .ansi-default-inverse-fg {
  color: var(--jp-ui-inverse-font-color0);
}
.jp-RenderedText pre .ansi-default-inverse-bg {
  background-color: var(--jp-inverse-layout-color0);
  padding: var(--jp-private-code-span-padding) 0;
}

.jp-RenderedText pre .ansi-bold {
  font-weight: bold;
}
.jp-RenderedText pre .ansi-underline {
  text-decoration: underline;
}

.jp-RenderedText[data-mime-type='application/vnd.jupyter.stderr'] {
  background: var(--jp-rendermime-error-background);
  padding-top: var(--jp-code-padding);
}

/*-----------------------------------------------------------------------------
| RenderedLatex
|----------------------------------------------------------------------------*/

.jp-RenderedLatex {
  color: var(--jp-content-font-color1);
  font-size: var(--jp-content-font-size1);
  line-height: var(--jp-content-line-height);
}

/* Left-justify outputs.*/
.jp-OutputArea-output.jp-RenderedLatex {
  padding: var(--jp-code-padding);
  text-align: left;
}

/*-----------------------------------------------------------------------------
| RenderedHTML
|----------------------------------------------------------------------------*/

.jp-RenderedHTMLCommon {
  color: var(--jp-content-font-color1);
  font-family: var(--jp-content-font-family);
  font-size: var(--jp-content-font-size1);
  line-height: var(--jp-content-line-height);
  /* Give a bit more R padding on Markdown text to keep line lengths reasonable */
  padding-right: 20px;
}

.jp-RenderedHTMLCommon em {
  font-style: italic;
}

.jp-RenderedHTMLCommon strong {
  font-weight: bold;
}

.jp-RenderedHTMLCommon u {
  text-decoration: underline;
}

.jp-RenderedHTMLCommon a:link {
  text-decoration: none;
  color: var(--jp-content-link-color);
}

.jp-RenderedHTMLCommon a:hover {
  text-decoration: underline;
  color: var(--jp-content-link-color);
}

.jp-RenderedHTMLCommon a:visited {
  text-decoration: none;
  color: var(--jp-content-link-color);
}

/* Headings */

.jp-RenderedHTMLCommon h1,
.jp-RenderedHTMLCommon h2,
.jp-RenderedHTMLCommon h3,
.jp-RenderedHTMLCommon h4,
.jp-RenderedHTMLCommon h5,
.jp-RenderedHTMLCommon h6 {
  line-height: var(--jp-content-heading-line-height);
  font-weight: var(--jp-content-heading-font-weight);
  font-style: normal;
  margin: var(--jp-content-heading-margin-top) 0
    var(--jp-content-heading-margin-bottom) 0;
}

.jp-RenderedHTMLCommon h1:first-child,
.jp-RenderedHTMLCommon h2:first-child,
.jp-RenderedHTMLCommon h3:first-child,
.jp-RenderedHTMLCommon h4:first-child,
.jp-RenderedHTMLCommon h5:first-child,
.jp-RenderedHTMLCommon h6:first-child {
  margin-top: calc(0.5 * var(--jp-content-heading-margin-top));
}

.jp-RenderedHTMLCommon h1:last-child,
.jp-RenderedHTMLCommon h2:last-child,
.jp-RenderedHTMLCommon h3:last-child,
.jp-RenderedHTMLCommon h4:last-child,
.jp-RenderedHTMLCommon h5:last-child,
.jp-RenderedHTMLCommon h6:last-child {
  margin-bottom: calc(0.5 * var(--jp-content-heading-margin-bottom));
}

.jp-RenderedHTMLCommon h1 {
  font-size: var(--jp-content-font-size5);
}

.jp-RenderedHTMLCommon h2 {
  font-size: var(--jp-content-font-size4);
}

.jp-RenderedHTMLCommon h3 {
  font-size: var(--jp-content-font-size3);
}

.jp-RenderedHTMLCommon h4 {
  font-size: var(--jp-content-font-size2);
}

.jp-RenderedHTMLCommon h5 {
  font-size: var(--jp-content-font-size1);
}

.jp-RenderedHTMLCommon h6 {
  font-size: var(--jp-content-font-size0);
}

/* Lists */

.jp-RenderedHTMLCommon ul:not(.list-inline),
.jp-RenderedHTMLCommon ol:not(.list-inline) {
  padding-left: 2em;
}

.jp-RenderedHTMLCommon ul {
  list-style: disc;
}

.jp-RenderedHTMLCommon ul ul {
  list-style: square;
}

.jp-RenderedHTMLCommon ul ul ul {
  list-style: circle;
}

.jp-RenderedHTMLCommon ol {
  list-style: decimal;
}

.jp-RenderedHTMLCommon ol ol {
  list-style: upper-alpha;
}

.jp-RenderedHTMLCommon ol ol ol {
  list-style: lower-alpha;
}

.jp-RenderedHTMLCommon ol ol ol ol {
  list-style: lower-roman;
}

.jp-RenderedHTMLCommon ol ol ol ol ol {
  list-style: decimal;
}

.jp-RenderedHTMLCommon ol,
.jp-RenderedHTMLCommon ul {
  margin-bottom: 1em;
}

.jp-RenderedHTMLCommon ul ul,
.jp-RenderedHTMLCommon ul ol,
.jp-RenderedHTMLCommon ol ul,
.jp-RenderedHTMLCommon ol ol {
  margin-bottom: 0em;
}

.jp-RenderedHTMLCommon hr {
  color: var(--jp-border-color2);
  background-color: var(--jp-border-color1);
  margin-top: 1em;
  margin-bottom: 1em;
}

.jp-RenderedHTMLCommon > pre {
  margin: 1.5em 2em;
}

.jp-RenderedHTMLCommon pre,
.jp-RenderedHTMLCommon code {
  border: 0;
  background-color: var(--jp-layout-color0);
  color: var(--jp-content-font-color1);
  font-family: var(--jp-code-font-family);
  font-size: inherit;
  line-height: var(--jp-code-line-height);
  padding: 0;
  white-space: pre-wrap;
}

.jp-RenderedHTMLCommon :not(pre) > code {
  background-color: var(--jp-layout-color2);
  padding: 1px 5px;
}

/* Tables */

.jp-RenderedHTMLCommon table {
  border-collapse: collapse;
  border-spacing: 0;
  border: none;
  color: var(--jp-ui-font-color1);
  font-size: 12px;
  table-layout: fixed;
  margin-left: auto;
  margin-right: auto;
}

.jp-RenderedHTMLCommon thead {
  border-bottom: var(--jp-border-width) solid var(--jp-border-color1);
  vertical-align: bottom;
}

.jp-RenderedHTMLCommon td,
.jp-RenderedHTMLCommon th,
.jp-RenderedHTMLCommon tr {
  vertical-align: middle;
  padding: 0.5em 0.5em;
  line-height: normal;
  white-space: normal;
  max-width: none;
  border: none;
}

.jp-RenderedMarkdown.jp-RenderedHTMLCommon td,
.jp-RenderedMarkdown.jp-RenderedHTMLCommon th {
  max-width: none;
}

:not(.jp-RenderedMarkdown).jp-RenderedHTMLCommon td,
:not(.jp-RenderedMarkdown).jp-RenderedHTMLCommon th,
:not(.jp-RenderedMarkdown).jp-RenderedHTMLCommon tr {
  text-align: right;
}

.jp-RenderedHTMLCommon th {
  font-weight: bold;
}

.jp-RenderedHTMLCommon tbody tr:nth-child(odd) {
  background: var(--jp-layout-color0);
}

.jp-RenderedHTMLCommon tbody tr:nth-child(even) {
  background: var(--jp-rendermime-table-row-background);
}

.jp-RenderedHTMLCommon tbody tr:hover {
  background: var(--jp-rendermime-table-row-hover-background);
}

.jp-RenderedHTMLCommon table {
  margin-bottom: 1em;
}

.jp-RenderedHTMLCommon p {
  text-align: left;
  margin: 0px;
}

.jp-RenderedHTMLCommon p {
  margin-bottom: 1em;
}

.jp-RenderedHTMLCommon img {
  -moz-force-broken-image-icon: 1;
}

/* Restrict to direct children as other images could be nested in other content. */
.jp-RenderedHTMLCommon > img {
  display: block;
  margin-left: 0;
  margin-right: 0;
  margin-bottom: 1em;
}

/* Change color behind transparent images if they need it... */
[data-jp-theme-light='false'] .jp-RenderedImage img.jp-needs-light-background {
  background-color: var(--jp-inverse-layout-color1);
}
[data-jp-theme-light='true'] .jp-RenderedImage img.jp-needs-dark-background {
  background-color: var(--jp-inverse-layout-color1);
}
/* ...or leave it untouched if they don't */
[data-jp-theme-light='false'] .jp-RenderedImage img.jp-needs-dark-background {
}
[data-jp-theme-light='true'] .jp-RenderedImage img.jp-needs-light-background {
}

.jp-RenderedHTMLCommon img,
.jp-RenderedImage img,
.jp-RenderedHTMLCommon svg,
.jp-RenderedSVG svg {
  max-width: 100%;
  height: auto;
}

.jp-RenderedHTMLCommon img.jp-mod-unconfined,
.jp-RenderedImage img.jp-mod-unconfined,
.jp-RenderedHTMLCommon svg.jp-mod-unconfined,
.jp-RenderedSVG svg.jp-mod-unconfined {
  max-width: none;
}

.jp-RenderedHTMLCommon .alert {
  padding: var(--jp-notebook-padding);
  border: var(--jp-border-width) solid transparent;
  border-radius: var(--jp-border-radius);
  margin-bottom: 1em;
}

.jp-RenderedHTMLCommon .alert-info {
  color: var(--jp-info-color0);
  background-color: var(--jp-info-color3);
  border-color: var(--jp-info-color2);
}
.jp-RenderedHTMLCommon .alert-info hr {
  border-color: var(--jp-info-color3);
}
.jp-RenderedHTMLCommon .alert-info > p:last-child,
.jp-RenderedHTMLCommon .alert-info > ul:last-child {
  margin-bottom: 0;
}

.jp-RenderedHTMLCommon .alert-warning {
  color: var(--jp-warn-color0);
  background-color: var(--jp-warn-color3);
  border-color: var(--jp-warn-color2);
}
.jp-RenderedHTMLCommon .alert-warning hr {
  border-color: var(--jp-warn-color3);
}
.jp-RenderedHTMLCommon .alert-warning > p:last-child,
.jp-RenderedHTMLCommon .alert-warning > ul:last-child {
  margin-bottom: 0;
}

.jp-RenderedHTMLCommon .alert-success {
  color: var(--jp-success-color0);
  background-color: var(--jp-success-color3);
  border-color: var(--jp-success-color2);
}
.jp-RenderedHTMLCommon .alert-success hr {
  border-color: var(--jp-success-color3);
}
.jp-RenderedHTMLCommon .alert-success > p:last-child,
.jp-RenderedHTMLCommon .alert-success > ul:last-child {
  margin-bottom: 0;
}

.jp-RenderedHTMLCommon .alert-danger {
  color: var(--jp-error-color0);
  background-color: var(--jp-error-color3);
  border-color: var(--jp-error-color2);
}
.jp-RenderedHTMLCommon .alert-danger hr {
  border-color: var(--jp-error-color3);
}
.jp-RenderedHTMLCommon .alert-danger > p:last-child,
.jp-RenderedHTMLCommon .alert-danger > ul:last-child {
  margin-bottom: 0;
}

.jp-RenderedHTMLCommon blockquote {
  margin: 1em 2em;
  padding: 0 1em;
  border-left: 5px solid var(--jp-border-color2);
}

a.jp-InternalAnchorLink {
  visibility: hidden;
  margin-left: 8px;
  color: var(--md-blue-800);
}

h1:hover .jp-InternalAnchorLink,
h2:hover .jp-InternalAnchorLink,
h3:hover .jp-InternalAnchorLink,
h4:hover .jp-InternalAnchorLink,
h5:hover .jp-InternalAnchorLink,
h6:hover .jp-InternalAnchorLink {
  visibility: visible;
}

.jp-RenderedHTMLCommon kbd {
  background-color: var(--jp-rendermime-table-row-background);
  border: 1px solid var(--jp-border-color0);
  border-bottom-color: var(--jp-border-color2);
  border-radius: 3px;
  box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.25);
  display: inline-block;
  font-size: 0.8em;
  line-height: 1em;
  padding: 0.2em 0.5em;
}

/* Most direct children of .jp-RenderedHTMLCommon have a margin-bottom of 1.0.
 * At the bottom of cells this is a bit too much as there is also spacing
 * between cells. Going all the way to 0 gets too tight between markdown and
 * code cells.
 */
.jp-RenderedHTMLCommon > *:last-child {
  margin-bottom: 0.5em;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-MimeDocument {
  outline: none;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Variables
|----------------------------------------------------------------------------*/

:root {
  --jp-private-filebrowser-button-height: 28px;
  --jp-private-filebrowser-button-width: 48px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-FileBrowser {
  display: flex;
  flex-direction: column;
  color: var(--jp-ui-font-color1);
  background: var(--jp-layout-color1);
  /* This is needed so that all font sizing of children done in ems is
   * relative to this base size */
  font-size: var(--jp-ui-font-size1);
}

.jp-FileBrowser-toolbar.jp-Toolbar {
  border-bottom: none;
  height: auto;
  margin: var(--jp-toolbar-header-margin);
  box-shadow: none;
}

.jp-BreadCrumbs {
  flex: 0 0 auto;
  margin: 8px 12px 8px 12px;
}

.jp-BreadCrumbs-item {
  margin: 0px 2px;
  padding: 0px 2px;
  border-radius: var(--jp-border-radius);
  cursor: pointer;
}

.jp-BreadCrumbs-item:hover {
  background-color: var(--jp-layout-color2);
}

.jp-BreadCrumbs-item:first-child {
  margin-left: 0px;
}

.jp-BreadCrumbs-item.jp-mod-dropTarget {
  background-color: var(--jp-brand-color2);
  opacity: 0.7;
}

/*-----------------------------------------------------------------------------
| Buttons
|----------------------------------------------------------------------------*/

.jp-FileBrowser-toolbar.jp-Toolbar {
  padding: 0px;
  margin: 8px 12px 0px 12px;
}

.jp-FileBrowser-toolbar.jp-Toolbar {
  justify-content: flex-start;
}

.jp-FileBrowser-toolbar.jp-Toolbar .jp-Toolbar-item {
  flex: 0 0 auto;
  padding-left: 0px;
  padding-right: 2px;
}

.jp-FileBrowser-toolbar.jp-Toolbar .jp-ToolbarButtonComponent {
  width: 40px;
}

.jp-FileBrowser-toolbar.jp-Toolbar
  .jp-Toolbar-item:first-child
  .jp-ToolbarButtonComponent {
  width: 72px;
  background: var(--jp-brand-color1);
}

.jp-FileBrowser-toolbar.jp-Toolbar
  .jp-Toolbar-item:first-child
  .jp-ToolbarButtonComponent:focus-visible {
  background-color: var(--jp-brand-color0);
}

.jp-FileBrowser-toolbar.jp-Toolbar
  .jp-Toolbar-item:first-child
  .jp-ToolbarButtonComponent
  .jp-icon3 {
  fill: white;
}

/*-----------------------------------------------------------------------------
| Other styles
|----------------------------------------------------------------------------*/

.jp-FileDialog.jp-mod-conflict input {
  color: var(--jp-error-color1);
}

.jp-FileDialog .jp-new-name-title {
  margin-top: 12px;
}

.jp-LastModified-hidden {
  display: none;
}

.jp-FileBrowser-filterBox {
  padding: 0px;
  flex: 0 0 auto;
  margin: 8px 12px 0px 12px;
}

/*-----------------------------------------------------------------------------
| DirListing
|----------------------------------------------------------------------------*/

.jp-DirListing {
  flex: 1 1 auto;
  display: flex;
  flex-direction: column;
  outline: 0;
}

.jp-DirListing:focus-visible {
  border: 1px solid var(--jp-brand-color1);
}

.jp-DirListing-header {
  flex: 0 0 auto;
  display: flex;
  flex-direction: row;
  overflow: hidden;
  border-top: var(--jp-border-width) solid var(--jp-border-color2);
  border-bottom: var(--jp-border-width) solid var(--jp-border-color1);
  box-shadow: var(--jp-toolbar-box-shadow);
  z-index: 2;
}

.jp-DirListing-headerItem {
  padding: 4px 12px 2px 12px;
  font-weight: 500;
}

.jp-DirListing-headerItem:hover {
  background: var(--jp-layout-color2);
}

.jp-DirListing-headerItem.jp-id-name {
  flex: 1 0 84px;
}

.jp-DirListing-headerItem.jp-id-modified {
  flex: 0 0 112px;
  border-left: var(--jp-border-width) solid var(--jp-border-color2);
  text-align: right;
}

.jp-id-narrow {
  display: none;
  flex: 0 0 5px;
  padding: 4px 4px;
  border-left: var(--jp-border-width) solid var(--jp-border-color2);
  text-align: right;
  color: var(--jp-border-color2);
}

.jp-DirListing-narrow .jp-id-narrow {
  display: block;
}

.jp-DirListing-narrow .jp-id-modified,
.jp-DirListing-narrow .jp-DirListing-itemModified {
  display: none;
}

.jp-DirListing-headerItem.jp-mod-selected {
  font-weight: 600;
}

/* increase specificity to override bundled default */
.jp-DirListing-content {
  flex: 1 1 auto;
  margin: 0;
  padding: 0;
  list-style-type: none;
  overflow: auto;
  background-color: var(--jp-layout-color1);
}

.jp-DirListing-content mark {
  color: var(--jp-ui-font-color0);
  background-color: transparent;
  font-weight: bold;
}

.jp-DirListing-content .jp-DirListing-item.jp-mod-selected mark {
  color: var(--jp-ui-inverse-font-color0);
}

/* Style the directory listing content when a user drops a file to upload */
.jp-DirListing.jp-mod-native-drop .jp-DirListing-content {
  outline: 5px dashed rgba(128, 128, 128, 0.5);
  outline-offset: -10px;
  cursor: copy;
}

.jp-DirListing-item {
  display: flex;
  flex-direction: row;
  padding: 4px 12px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.jp-DirListing-item[data-is-dot] {
  opacity: 75%;
}

.jp-DirListing-item.jp-mod-selected {
  color: var(--jp-ui-inverse-font-color1);
  background: var(--jp-brand-color1);
}

.jp-DirListing-item.jp-mod-dropTarget {
  background: var(--jp-brand-color3);
}

.jp-DirListing-item:hover:not(.jp-mod-selected) {
  background: var(--jp-layout-color2);
}

.jp-DirListing-itemIcon {
  flex: 0 0 20px;
  margin-right: 4px;
}

.jp-DirListing-itemText {
  flex: 1 0 64px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  user-select: none;
}

.jp-DirListing-itemModified {
  flex: 0 0 125px;
  text-align: right;
}

.jp-DirListing-editor {
  flex: 1 0 64px;
  outline: none;
  border: none;
}

.jp-DirListing-item.jp-mod-running .jp-DirListing-itemIcon:before {
  color: var(--jp-success-color1);
  content: '\25CF';
  font-size: 8px;
  position: absolute;
  left: -8px;
}

.jp-DirListing-item.jp-mod-running.jp-mod-selected
  .jp-DirListing-itemIcon:before {
  color: var(--jp-ui-inverse-font-color1);
}

.jp-DirListing-item.lm-mod-drag-image,
.jp-DirListing-item.jp-mod-selected.lm-mod-drag-image {
  font-size: var(--jp-ui-font-size1);
  padding-left: 4px;
  margin-left: 4px;
  width: 160px;
  background-color: var(--jp-ui-inverse-font-color2);
  box-shadow: var(--jp-elevation-z2);
  border-radius: 0px;
  color: var(--jp-ui-font-color1);
  transform: translateX(-40%) translateY(-58%);
}

.jp-DirListing-deadSpace {
  flex: 1 1 auto;
  margin: 0;
  padding: 0;
  list-style-type: none;
  overflow: auto;
  background-color: var(--jp-layout-color1);
}

.jp-Document {
  min-width: 120px;
  min-height: 120px;
  outline: none;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Private CSS variables
|----------------------------------------------------------------------------*/

:root {
}

/*-----------------------------------------------------------------------------
| Main OutputArea
| OutputArea has a list of Outputs
|----------------------------------------------------------------------------*/

.jp-OutputArea {
  overflow-y: auto;
}

.jp-OutputArea-child {
  display: flex;
  flex-direction: row;
}

body[data-format='mobile'] .jp-OutputArea-child {
  flex-direction: column;
}

.jp-OutputPrompt {
  flex: 0 0 var(--jp-cell-prompt-width);
  color: var(--jp-cell-outprompt-font-color);
  font-family: var(--jp-cell-prompt-font-family);
  padding: var(--jp-code-padding);
  letter-spacing: var(--jp-cell-prompt-letter-spacing);
  line-height: var(--jp-code-line-height);
  font-size: var(--jp-code-font-size);
  border: var(--jp-border-width) solid transparent;
  opacity: var(--jp-cell-prompt-opacity);
  /* Right align prompt text, don't wrap to handle large prompt numbers */
  text-align: right;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  /* Disable text selection */
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

body[data-format='mobile'] .jp-OutputPrompt {
  flex: 0 0 auto;
  text-align: left;
}

.jp-OutputArea-output {
  height: auto;
  overflow: auto;
  user-select: text;
  -moz-user-select: text;
  -webkit-user-select: text;
  -ms-user-select: text;
}

.jp-OutputArea-child .jp-OutputArea-output {
  flex-grow: 1;
  flex-shrink: 1;
}

body[data-format='mobile'] .jp-OutputArea-child .jp-OutputArea-output {
  margin-left: var(--jp-notebook-padding);
}

/**
 * Isolated output.
 */
.jp-OutputArea-output.jp-mod-isolated {
  width: 100%;
  display: block;
}

/*
When drag events occur, `p-mod-override-cursor` is added to the body.
Because iframes steal all cursor events, the following two rules are necessary
to suppress pointer events while resize drags are occurring. There may be a
better solution to this problem.
*/
body.lm-mod-override-cursor .jp-OutputArea-output.jp-mod-isolated {
  position: relative;
}

body.lm-mod-override-cursor .jp-OutputArea-output.jp-mod-isolated:before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: transparent;
}

/* pre */

.jp-OutputArea-output pre {
  border: none;
  margin: 0px;
  padding: 0px;
  overflow-x: auto;
  overflow-y: auto;
  word-break: break-all;
  word-wrap: break-word;
  white-space: pre-wrap;
}

/* tables */

.jp-OutputArea-output.jp-RenderedHTMLCommon table {
  margin-left: 0;
  margin-right: 0;
}

/* description lists */

.jp-OutputArea-output dl,
.jp-OutputArea-output dt,
.jp-OutputArea-output dd {
  display: block;
}

.jp-OutputArea-output dl {
  width: 100%;
  overflow: hidden;
  padding: 0;
  margin: 0;
}

.jp-OutputArea-output dt {
  font-weight: bold;
  float: left;
  width: 20%;
  padding: 0;
  margin: 0;
}

.jp-OutputArea-output dd {
  float: left;
  width: 80%;
  padding: 0;
  margin: 0;
}

/* Hide the gutter in case of
 *  - nested output areas (e.g. in the case of output widgets)
 *  - mirrored output areas
 */
.jp-OutputArea .jp-OutputArea .jp-OutputArea-prompt {
  display: none;
}

/*-----------------------------------------------------------------------------
| executeResult is added to any Output-result for the display of the object
| returned by a cell
|----------------------------------------------------------------------------*/

.jp-OutputArea-output.jp-OutputArea-executeResult {
  margin-left: 0px;
  flex: 1 1 auto;
}

/* Text output with the Out[] prompt needs a top padding to match the
 * alignment of the Out[] prompt itself.
 */
.jp-OutputArea-executeResult .jp-RenderedText.jp-OutputArea-output {
  padding-top: var(--jp-code-padding);
  border-top: var(--jp-border-width) solid transparent;
}

/*-----------------------------------------------------------------------------
| The Stdin output
|----------------------------------------------------------------------------*/

.jp-OutputArea-stdin {
  line-height: var(--jp-code-line-height);
  padding-top: var(--jp-code-padding);
  display: flex;
}

.jp-Stdin-prompt {
  color: var(--jp-content-font-color0);
  padding-right: var(--jp-code-padding);
  vertical-align: baseline;
  flex: 0 0 auto;
}

.jp-Stdin-input {
  font-family: var(--jp-code-font-family);
  font-size: inherit;
  color: inherit;
  background-color: inherit;
  width: 42%;
  min-width: 200px;
  /* make sure input baseline aligns with prompt */
  vertical-align: baseline;
  /* padding + margin = 0.5em between prompt and cursor */
  padding: 0em 0.25em;
  margin: 0em 0.25em;
  flex: 0 0 70%;
}

.jp-Stdin-input:focus {
  box-shadow: none;
}

/*-----------------------------------------------------------------------------
| Output Area View
|----------------------------------------------------------------------------*/

.jp-LinkedOutputView .jp-OutputArea {
  height: 100%;
  display: block;
}

.jp-LinkedOutputView .jp-OutputArea-output:only-child {
  height: 100%;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

.jp-Collapser {
  flex: 0 0 var(--jp-cell-collapser-width);
  padding: 0px;
  margin: 0px;
  border: none;
  outline: none;
  background: transparent;
  border-radius: var(--jp-border-radius);
  opacity: 1;
}

.jp-Collapser-child {
  display: block;
  width: 100%;
  box-sizing: border-box;
  /* height: 100% doesn't work because the height of its parent is computed from content */
  position: absolute;
  top: 0px;
  bottom: 0px;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Header/Footer
|----------------------------------------------------------------------------*/

/* Hidden by zero height by default */
.jp-CellHeader,
.jp-CellFooter {
  height: 0px;
  width: 100%;
  padding: 0px;
  margin: 0px;
  border: none;
  outline: none;
  background: transparent;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Input
|----------------------------------------------------------------------------*/

/* All input areas */
.jp-InputArea {
  display: flex;
  flex-direction: row;
  overflow: hidden;
}

body[data-format='mobile'] .jp-InputArea {
  flex-direction: column;
}

.jp-InputArea-editor {
  flex: 1 1 auto;
  overflow: hidden;
}

.jp-InputArea-editor {
  /* This is the non-active, default styling */
  border: var(--jp-border-width) solid var(--jp-cell-editor-border-color);
  border-radius: 0px;
  background: var(--jp-cell-editor-background);
}

body[data-format='mobile'] .jp-InputArea-editor {
  margin-left: var(--jp-notebook-padding);
}

.jp-InputPrompt {
  flex: 0 0 var(--jp-cell-prompt-width);
  color: var(--jp-cell-inprompt-font-color);
  font-family: var(--jp-cell-prompt-font-family);
  padding: var(--jp-code-padding);
  letter-spacing: var(--jp-cell-prompt-letter-spacing);
  opacity: var(--jp-cell-prompt-opacity);
  line-height: var(--jp-code-line-height);
  font-size: var(--jp-code-font-size);
  border: var(--jp-border-width) solid transparent;
  opacity: var(--jp-cell-prompt-opacity);
  /* Right align prompt text, don't wrap to handle large prompt numbers */
  text-align: right;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  /* Disable text selection */
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

body[data-format='mobile'] .jp-InputPrompt {
  flex: 0 0 auto;
  text-align: left;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Placeholder
|----------------------------------------------------------------------------*/

.jp-Placeholder {
  display: flex;
  flex-direction: row;
  flex: 1 1 auto;
}

.jp-Placeholder-prompt {
  box-sizing: border-box;
}

.jp-Placeholder-content {
  flex: 1 1 auto;
  border: none;
  background: transparent;
  height: 20px;
  box-sizing: border-box;
}

.jp-Placeholder-content .jp-MoreHorizIcon {
  width: 32px;
  height: 16px;
  border: 1px solid transparent;
  border-radius: var(--jp-border-radius);
}

.jp-Placeholder-content .jp-MoreHorizIcon:hover {
  border: 1px solid var(--jp-border-color1);
  box-shadow: 0px 0px 2px 0px rgba(0, 0, 0, 0.25);
  background-color: var(--jp-layout-color0);
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Private CSS variables
|----------------------------------------------------------------------------*/

:root {
  --jp-private-cell-scrolling-output-offset: 5px;
}

/*-----------------------------------------------------------------------------
| Cell
|----------------------------------------------------------------------------*/

.jp-Cell {
  padding: var(--jp-cell-padding);
  margin: 0px;
  border: none;
  outline: none;
  background: transparent;
}

/*-----------------------------------------------------------------------------
| Common input/output
|----------------------------------------------------------------------------*/

.jp-Cell-inputWrapper,
.jp-Cell-outputWrapper {
  display: flex;
  flex-direction: row;
  padding: 0px;
  margin: 0px;
  /* Added to reveal the box-shadow on the input and output collapsers. */
  overflow: visible;
}

/* Only input/output areas inside cells */
.jp-Cell-inputArea,
.jp-Cell-outputArea {
  flex: 1 1 auto;
}

/*-----------------------------------------------------------------------------
| Collapser
|----------------------------------------------------------------------------*/

/* Make the output collapser disappear when there is not output, but do so
 * in a manner that leaves it in the layout and preserves its width.
 */
.jp-Cell.jp-mod-noOutputs .jp-Cell-outputCollapser {
  border: none !important;
  background: transparent !important;
}

.jp-Cell:not(.jp-mod-noOutputs) .jp-Cell-outputCollapser {
  min-height: var(--jp-cell-collapser-min-height);
}

/*-----------------------------------------------------------------------------
| Output
|----------------------------------------------------------------------------*/

/* Put a space between input and output when there IS output */
.jp-Cell:not(.jp-mod-noOutputs) .jp-Cell-outputWrapper {
  margin-top: 5px;
}

.jp-CodeCell.jp-mod-outputsScrolled .jp-Cell-outputArea {
  overflow-y: auto;
  max-height: 200px;
  box-shadow: inset 0 0 6px 2px rgba(0, 0, 0, 0.3);
  margin-left: var(--jp-private-cell-scrolling-output-offset);
}

.jp-CodeCell.jp-mod-outputsScrolled .jp-OutputArea-prompt {
  flex: 0 0
    calc(
      var(--jp-cell-prompt-width) -
        var(--jp-private-cell-scrolling-output-offset)
    );
}

/*-----------------------------------------------------------------------------
| CodeCell
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| MarkdownCell
|----------------------------------------------------------------------------*/

.jp-MarkdownOutput {
  flex: 1 1 auto;
  margin-top: 0;
  margin-bottom: 0;
  padding-left: var(--jp-code-padding);
}

.jp-MarkdownOutput.jp-RenderedHTMLCommon {
  overflow: auto;
}

.jp-showHiddenCellsButton {
  margin-left: calc(var(--jp-cell-prompt-width) + 2 * var(--jp-code-padding));
  margin-top: var(--jp-code-padding);
  border: 1px solid var(--jp-border-color2);
  background-color: var(--jp-border-color3) !important;
  color: var(--jp-content-font-color0) !important;
}

.jp-showHiddenCellsButton:hover {
  background-color: var(--jp-border-color2) !important;
}

.jp-collapseHeadingButton {
  display: none;
}

.jp-MarkdownCell:hover .jp-collapseHeadingButton {
  display: flex;
  min-height: var(--jp-cell-collapser-min-height);
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Variables
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------

/*-----------------------------------------------------------------------------
| Styles
|----------------------------------------------------------------------------*/

.jp-NotebookPanel-toolbar {
  padding: 2px;
}

.jp-Toolbar-item.jp-Notebook-toolbarCellType .jp-select-wrapper.jp-mod-focused {
  border: none;
  box-shadow: none;
}

.jp-Notebook-toolbarCellTypeDropdown select {
  height: 24px;
  font-size: var(--jp-ui-font-size1);
  line-height: 14px;
  border-radius: 0;
  display: block;
}

.jp-Notebook-toolbarCellTypeDropdown span {
  top: 5px !important;
}

/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Private CSS variables
|----------------------------------------------------------------------------*/

:root {
  --jp-private-notebook-dragImage-width: 304px;
  --jp-private-notebook-dragImage-height: 36px;
  --jp-private-notebook-selected-color: var(--md-blue-400);
  --jp-private-notebook-active-color: var(--md-green-400);
}

/*-----------------------------------------------------------------------------
| Imports
|----------------------------------------------------------------------------*/

/*-----------------------------------------------------------------------------
| Notebook
|----------------------------------------------------------------------------*/

.jp-NotebookPanel {
  display: block;
  height: 100%;
}

.jp-NotebookPanel.jp-Document {
  min-width: 240px;
  min-height: 120px;
}

.jp-Notebook {
  padding: var(--jp-notebook-padding);
  outline: none;
  overflow: auto;
  background: var(--jp-layout-color0);
}

.jp-Notebook.jp-mod-scrollPastEnd::after {
  display: block;
  content: '';
  min-height: var(--jp-notebook-scroll-padding);
}

.jp-MainAreaWidget-ContainStrict .jp-Notebook * {
  contain: strict;
}

.jp-Notebook-render * {
  contain: none !important;
}

.jp-Notebook .jp-Cell {
  overflow: visible;
}

.jp-Notebook .jp-Cell .jp-InputPrompt {
  cursor: move;
  float: left;
}

/*-----------------------------------------------------------------------------
| Notebook state related styling
|
| The notebook and cells each have states, here are the possibilities:
|
| - Notebook
|   - Command
|   - Edit
| - Cell
|   - None
|   - Active (only one can be active)
|   - Selected (the cells actions are applied to)
|   - Multiselected (when multiple selected, the cursor)
|   - No outputs
|----------------------------------------------------------------------------*/

/* Command or edit modes */

.jp-Notebook .jp-Cell:not(.jp-mod-active) .jp-InputPrompt {
  opacity: var(--jp-cell-prompt-not-active-opacity);
  color: var(--jp-cell-prompt-not-active-font-color);
}

.jp-Notebook .jp-Cell:not(.jp-mod-active) .jp-OutputPrompt {
  opacity: var(--jp-cell-prompt-not-active-opacity);
  color: var(--jp-cell-prompt-not-active-font-color);
}

/* cell is active */
.jp-Notebook .jp-Cell.jp-mod-active .jp-Collapser {
  background: var(--jp-brand-color1);
}

/* cell is dirty */
.jp-Notebook .jp-Cell.jp-mod-dirty .jp-InputPrompt {
  color: var(--jp-warn-color1);
}
.jp-Notebook .jp-Cell.jp-mod-dirty .jp-InputPrompt:before {
  color: var(--jp-warn-color1);
  content: '•';
}

.jp-Notebook .jp-Cell.jp-mod-active.jp-mod-dirty .jp-Collapser {
  background: var(--jp-warn-color1);
}

/* collapser is hovered */
.jp-Notebook .jp-Cell .jp-Collapser:hover {
  box-shadow: var(--jp-elevation-z2);
  background: var(--jp-brand-color1);
  opacity: var(--jp-cell-collapser-not-active-hover-opacity);
}

/* cell is active and collapser is hovered */
.jp-Notebook .jp-Cell.jp-mod-active .jp-Collapser:hover {
  background: var(--jp-brand-color0);
  opacity: 1;
}

/* Command mode */

.jp-Notebook.jp-mod-commandMode .jp-Cell.jp-mod-selected {
  background: var(--jp-notebook-multiselected-color);
}

.jp-Notebook.jp-mod-commandMode
  .jp-Cell.jp-mod-active.jp-mod-selected:not(.jp-mod-multiSelected) {
  background: transparent;
}

/* Edit mode */

.jp-Notebook.jp-mod-editMode .jp-Cell.jp-mod-active .jp-InputArea-editor {
  border: var(--jp-border-width) solid var(--jp-cell-editor-active-border-color);
  box-shadow: var(--jp-input-box-shadow);
  background-color: var(--jp-cell-editor-active-background);
}

/*-----------------------------------------------------------------------------
| Notebook drag and drop
|----------------------------------------------------------------------------*/

.jp-Notebook-cell.jp-mod-dropSource {
  opacity: 0.5;
}

.jp-Notebook-cell.jp-mod-dropTarget,
.jp-Notebook.jp-mod-commandMode
  .jp-Notebook-cell.jp-mod-active.jp-mod-selected.jp-mod-dropTarget {
  border-top-color: var(--jp-private-notebook-selected-color);
  border-top-style: solid;
  border-top-width: 2px;
}

.jp-dragImage {
  display: block;
  flex-direction: row;
  width: var(--jp-private-notebook-dragImage-width);
  height: var(--jp-private-notebook-dragImage-height);
  border: var(--jp-border-width) solid var(--jp-cell-editor-border-color);
  background: var(--jp-cell-editor-background);
  overflow: visible;
}

.jp-dragImage-singlePrompt {
  box-shadow: 2px 2px 4px 0px rgba(0, 0, 0, 0.12);
}

.jp-dragImage .jp-dragImage-content {
  flex: 1 1 auto;
  z-index: 2;
  font-size: var(--jp-code-font-size);
  font-family: var(--jp-code-font-family);
  line-height: var(--jp-code-line-height);
  padding: var(--jp-code-padding);
  border: var(--jp-border-width) solid var(--jp-cell-editor-border-color);
  background: var(--jp-cell-editor-background-color);
  color: var(--jp-content-font-color3);
  text-align: left;
  margin: 4px 4px 4px 0px;
}

.jp-dragImage .jp-dragImage-prompt {
  flex: 0 0 auto;
  min-width: 36px;
  color: var(--jp-cell-inprompt-font-color);
  padding: var(--jp-code-padding);
  padding-left: 12px;
  font-family: var(--jp-cell-prompt-font-family);
  letter-spacing: var(--jp-cell-prompt-letter-spacing);
  line-height: 1.9;
  font-size: var(--jp-code-font-size);
  border: var(--jp-border-width) solid transparent;
}

.jp-dragImage-multipleBack {
  z-index: -1;
  position: absolute;
  height: 32px;
  width: 300px;
  top: 8px;
  left: 8px;
  background: var(--jp-layout-color2);
  border: var(--jp-border-width) solid var(--jp-input-border-color);
  box-shadow: 2px 2px 4px 0px rgba(0, 0, 0, 0.12);
}

/*-----------------------------------------------------------------------------
| Cell toolbar
|----------------------------------------------------------------------------*/

.jp-NotebookTools {
  display: block;
  min-width: var(--jp-sidebar-min-width);
  color: var(--jp-ui-font-color1);
  background: var(--jp-layout-color1);
  /* This is needed so that all font sizing of children done in ems is
    * relative to this base size */
  font-size: var(--jp-ui-font-size1);
  overflow: auto;
}

.jp-NotebookTools-tool {
  padding: 0px 12px 0 12px;
}

.jp-ActiveCellTool {
  padding: 12px;
  background-color: var(--jp-layout-color1);
  border-top: none !important;
}

.jp-ActiveCellTool .jp-InputArea-prompt {
  flex: 0 0 auto;
  padding-left: 0px;
}

.jp-ActiveCellTool .jp-InputArea-editor {
  flex: 1 1 auto;
  background: var(--jp-cell-editor-background);
  border-color: var(--jp-cell-editor-border-color);
}

.jp-ActiveCellTool .jp-InputArea-editor .CodeMirror {
  background: transparent;
}

.jp-MetadataEditorTool {
  flex-direction: column;
  padding: 12px 0px 12px 0px;
}

.jp-RankedPanel > :not(:first-child) {
  margin-top: 12px;
}

.jp-KeySelector select.jp-mod-styled {
  font-size: var(--jp-ui-font-size1);
  color: var(--jp-ui-font-color0);
  border: var(--jp-border-width) solid var(--jp-border-color1);
}

.jp-KeySelector label,
.jp-MetadataEditorTool label {
  line-height: 1.4;
}

.jp-NotebookTools .jp-select-wrapper {
  margin-top: 4px;
  margin-bottom: 0px;
}

.jp-NotebookTools .jp-Collapse {
  margin-top: 16px;
}

/*-----------------------------------------------------------------------------
| Presentation Mode (.jp-mod-presentationMode)
|----------------------------------------------------------------------------*/

.jp-mod-presentationMode .jp-Notebook {
  --jp-content-font-size1: var(--jp-content-presentation-font-size1);
  --jp-code-font-size: var(--jp-code-presentation-font-size);
}

.jp-mod-presentationMode .jp-Notebook .jp-Cell .jp-InputPrompt,
.jp-mod-presentationMode .jp-Notebook .jp-Cell .jp-OutputPrompt {
  flex: 0 0 110px;
}

/*-----------------------------------------------------------------------------
| Placeholder
|----------------------------------------------------------------------------*/

.jp-Cell-Placeholder {
  padding-left: 55px;
}

.jp-Cell-Placeholder-wrapper {
  background: #fff;
  border: 1px solid;
  border-color: #e5e6e9 #dfe0e4 #d0d1d5;
  border-radius: 4px;
  -webkit-border-radius: 4px;
  margin: 10px 15px;
}

.jp-Cell-Placeholder-wrapper-inner {
  padding: 15px;
  position: relative;
}

.jp-Cell-Placeholder-wrapper-body {
  background-repeat: repeat;
  background-size: 50% auto;
}

.jp-Cell-Placeholder-wrapper-body div {
  background: #f6f7f8;
  background-image: -webkit-linear-gradient(
    left,
    #f6f7f8 0%,
    #edeef1 20%,
    #f6f7f8 40%,
    #f6f7f8 100%
  );
  background-repeat: no-repeat;
  background-size: 800px 104px;
  height: 104px;
  position: relative;
}

.jp-Cell-Placeholder-wrapper-body div {
  position: absolute;
  right: 15px;
  left: 15px;
  top: 15px;
}

div.jp-Cell-Placeholder-h1 {
  top: 20px;
  height: 20px;
  left: 15px;
  width: 150px;
}

div.jp-Cell-Placeholder-h2 {
  left: 15px;
  top: 50px;
  height: 10px;
  width: 100px;
}

div.jp-Cell-Placeholder-content-1,
div.jp-Cell-Placeholder-content-2,
div.jp-Cell-Placeholder-content-3 {
  left: 15px;
  right: 15px;
  height: 10px;
}

div.jp-Cell-Placeholder-content-1 {
  top: 100px;
}

div.jp-Cell-Placeholder-content-2 {
  top: 120px;
}

div.jp-Cell-Placeholder-content-3 {
  top: 140px;
}

</style> <style type="text/css">
/*-----------------------------------------------------------------------------
| Copyright (c) Jupyter Development Team.
| Distributed under the terms of the Modified BSD License.
|----------------------------------------------------------------------------*/

/*
The following CSS variables define the main, public API for styling JupyterLab.
These variables should be used by all plugins wherever possible. In other
words, plugins should not define custom colors, sizes, etc unless absolutely
necessary. This enables users to change the visual theme of JupyterLab
by changing these variables.

Many variables appear in an ordered sequence (0,1,2,3). These sequences
are designed to work well together, so for example, `--jp-border-color1` should
be used with `--jp-layout-color1`. The numbers have the following meanings:

* 0: super-primary, reserved for special emphasis
* 1: primary, most important under normal situations
* 2: secondary, next most important under normal situations
* 3: tertiary, next most important under normal situations

Throughout JupyterLab, we are mostly following principles from Google's
Material Design when selecting colors. We are not, however, following
all of MD as it is not optimized for dense, information rich UIs.
*/

:root {
  /* Elevation
   *
   * We style box-shadows using Material Design's idea of elevation. These particular numbers are taken from here:
   *
   * https://github.com/material-components/material-components-web
   * https://material-components-web.appspot.com/elevation.html
   */

  --jp-shadow-base-lightness: 0;
  --jp-shadow-umbra-color: rgba(
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    0.2
  );
  --jp-shadow-penumbra-color: rgba(
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    0.14
  );
  --jp-shadow-ambient-color: rgba(
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    var(--jp-shadow-base-lightness),
    0.12
  );
  --jp-elevation-z0: none;
  --jp-elevation-z1: 0px 2px 1px -1px var(--jp-shadow-umbra-color),
    0px 1px 1px 0px var(--jp-shadow-penumbra-color),
    0px 1px 3px 0px var(--jp-shadow-ambient-color);
  --jp-elevation-z2: 0px 3px 1px -2px var(--jp-shadow-umbra-color),
    0px 2px 2px 0px var(--jp-shadow-penumbra-color),
    0px 1px 5px 0px var(--jp-shadow-ambient-color);
  --jp-elevation-z4: 0px 2px 4px -1px var(--jp-shadow-umbra-color),
    0px 4px 5px 0px var(--jp-shadow-penumbra-color),
    0px 1px 10px 0px var(--jp-shadow-ambient-color);
  --jp-elevation-z6: 0px 3px 5px -1px var(--jp-shadow-umbra-color),
    0px 6px 10px 0px var(--jp-shadow-penumbra-color),
    0px 1px 18px 0px var(--jp-shadow-ambient-color);
  --jp-elevation-z8: 0px 5px 5px -3px var(--jp-shadow-umbra-color),
    0px 8px 10px 1px var(--jp-shadow-penumbra-color),
    0px 3px 14px 2px var(--jp-shadow-ambient-color);
  --jp-elevation-z12: 0px 7px 8px -4px var(--jp-shadow-umbra-color),
    0px 12px 17px 2px var(--jp-shadow-penumbra-color),
    0px 5px 22px 4px var(--jp-shadow-ambient-color);
  --jp-elevation-z16: 0px 8px 10px -5px var(--jp-shadow-umbra-color),
    0px 16px 24px 2px var(--jp-shadow-penumbra-color),
    0px 6px 30px 5px var(--jp-shadow-ambient-color);
  --jp-elevation-z20: 0px 10px 13px -6px var(--jp-shadow-umbra-color),
    0px 20px 31px 3px var(--jp-shadow-penumbra-color),
    0px 8px 38px 7px var(--jp-shadow-ambient-color);
  --jp-elevation-z24: 0px 11px 15px -7px var(--jp-shadow-umbra-color),
    0px 24px 38px 3px var(--jp-shadow-penumbra-color),
    0px 9px 46px 8px var(--jp-shadow-ambient-color);

  /* Borders
   *
   * The following variables, specify the visual styling of borders in JupyterLab.
   */

  --jp-border-width: 1px;
  --jp-border-color0: var(--md-grey-400);
  --jp-border-color1: var(--md-grey-400);
  --jp-border-color2: var(--md-grey-300);
  --jp-border-color3: var(--md-grey-200);
  --jp-border-radius: 2px;

  /* UI Fonts
   *
   * The UI font CSS variables are used for the typography all of the JupyterLab
   * user interface elements that are not directly user generated content.
   *
   * The font sizing here is done assuming that the body font size of --jp-ui-font-size1
   * is applied to a parent element. When children elements, such as headings, are sized
   * in em all things will be computed relative to that body size.
   */

  --jp-ui-font-scale-factor: 1.2;
  --jp-ui-font-size0: 0.83333em;
  --jp-ui-font-size1: 13px; /* Base font size */
  --jp-ui-font-size2: 1.2em;
  --jp-ui-font-size3: 1.44em;

  --jp-ui-font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica,
    Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol';

  /*
   * Use these font colors against the corresponding main layout colors.
   * In a light theme, these go from dark to light.
   */

  /* Defaults use Material Design specification */
  --jp-ui-font-color0: rgba(0, 0, 0, 1);
  --jp-ui-font-color1: rgba(0, 0, 0, 0.87);
  --jp-ui-font-color2: rgba(0, 0, 0, 0.54);
  --jp-ui-font-color3: rgba(0, 0, 0, 0.38);

  /*
   * Use these against the brand/accent/warn/error colors.
   * These will typically go from light to darker, in both a dark and light theme.
   */

  --jp-ui-inverse-font-color0: rgba(255, 255, 255, 1);
  --jp-ui-inverse-font-color1: rgba(255, 255, 255, 1);
  --jp-ui-inverse-font-color2: rgba(255, 255, 255, 0.7);
  --jp-ui-inverse-font-color3: rgba(255, 255, 255, 0.5);

  /* Content Fonts
   *
   * Content font variables are used for typography of user generated content.
   *
   * The font sizing here is done assuming that the body font size of --jp-content-font-size1
   * is applied to a parent element. When children elements, such as headings, are sized
   * in em all things will be computed relative to that body size.
   */

  --jp-content-line-height: 1.6;
  --jp-content-font-scale-factor: 1.2;
  --jp-content-font-size0: 0.83333em;
  --jp-content-font-size1: 14px; /* Base font size */
  --jp-content-font-size2: 1.2em;
  --jp-content-font-size3: 1.44em;
  --jp-content-font-size4: 1.728em;
  --jp-content-font-size5: 2.0736em;

  /* This gives a magnification of about 125% in presentation mode over normal. */
  --jp-content-presentation-font-size1: 17px;

  --jp-content-heading-line-height: 1;
  --jp-content-heading-margin-top: 1.2em;
  --jp-content-heading-margin-bottom: 0.8em;
  --jp-content-heading-font-weight: 500;

  /* Defaults use Material Design specification */
  --jp-content-font-color0: rgba(0, 0, 0, 1);
  --jp-content-font-color1: rgba(0, 0, 0, 0.87);
  --jp-content-font-color2: rgba(0, 0, 0, 0.54);
  --jp-content-font-color3: rgba(0, 0, 0, 0.38);

  --jp-content-link-color: var(--md-blue-700);

  --jp-content-font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji',
    'Segoe UI Symbol';

  /*
   * Code Fonts
   *
   * Code font variables are used for typography of code and other monospaces content.
   */

  --jp-code-font-size: 13px;
  --jp-code-line-height: 1.3077; /* 17px for 13px base */
  --jp-code-padding: 5px; /* 5px for 13px base, codemirror highlighting needs integer px value */
  --jp-code-font-family-default: Menlo, Consolas, 'DejaVu Sans Mono', monospace;
  --jp-code-font-family: var(--jp-code-font-family-default);

  /* This gives a magnification of about 125% in presentation mode over normal. */
  --jp-code-presentation-font-size: 16px;

  /* may need to tweak cursor width if you change font size */
  --jp-code-cursor-width0: 1.4px;
  --jp-code-cursor-width1: 2px;
  --jp-code-cursor-width2: 4px;

  /* Layout
   *
   * The following are the main layout colors use in JupyterLab. In a light
   * theme these would go from light to dark.
   */

  --jp-layout-color0: white;
  --jp-layout-color1: white;
  --jp-layout-color2: var(--md-grey-200);
  --jp-layout-color3: var(--md-grey-400);
  --jp-layout-color4: var(--md-grey-600);

  /* Inverse Layout
   *
   * The following are the inverse layout colors use in JupyterLab. In a light
   * theme these would go from dark to light.
   */

  --jp-inverse-layout-color0: #111111;
  --jp-inverse-layout-color1: var(--md-grey-900);
  --jp-inverse-layout-color2: var(--md-grey-800);
  --jp-inverse-layout-color3: var(--md-grey-700);
  --jp-inverse-layout-color4: var(--md-grey-600);

  /* Brand/accent */

  --jp-brand-color0: var(--md-blue-900);
  --jp-brand-color1: var(--md-blue-700);
  --jp-brand-color2: var(--md-blue-300);
  --jp-brand-color3: var(--md-blue-100);
  --jp-brand-color4: var(--md-blue-50);

  --jp-accent-color0: var(--md-green-900);
  --jp-accent-color1: var(--md-green-700);
  --jp-accent-color2: var(--md-green-300);
  --jp-accent-color3: var(--md-green-100);

  /* State colors (warn, error, success, info) */

  --jp-warn-color0: var(--md-orange-900);
  --jp-warn-color1: var(--md-orange-700);
  --jp-warn-color2: var(--md-orange-300);
  --jp-warn-color3: var(--md-orange-100);

  --jp-error-color0: var(--md-red-900);
  --jp-error-color1: var(--md-red-700);
  --jp-error-color2: var(--md-red-300);
  --jp-error-color3: var(--md-red-100);

  --jp-success-color0: var(--md-green-900);
  --jp-success-color1: var(--md-green-700);
  --jp-success-color2: var(--md-green-300);
  --jp-success-color3: var(--md-green-100);

  --jp-info-color0: var(--md-cyan-900);
  --jp-info-color1: var(--md-cyan-700);
  --jp-info-color2: var(--md-cyan-300);
  --jp-info-color3: var(--md-cyan-100);

  /* Cell specific styles */

  --jp-cell-padding: 5px;

  --jp-cell-collapser-width: 8px;
  --jp-cell-collapser-min-height: 20px;
  --jp-cell-collapser-not-active-hover-opacity: 0.6;

  --jp-cell-editor-background: var(--md-grey-100);
  --jp-cell-editor-border-color: var(--md-grey-300);
  --jp-cell-editor-box-shadow: inset 0 0 2px var(--md-blue-300);
  --jp-cell-editor-active-background: var(--jp-layout-color0);
  --jp-cell-editor-active-border-color: var(--jp-brand-color1);

  --jp-cell-prompt-width: 64px;
  --jp-cell-prompt-font-family: var(--jp-code-font-family-default);
  --jp-cell-prompt-letter-spacing: 0px;
  --jp-cell-prompt-opacity: 1;
  --jp-cell-prompt-not-active-opacity: 0.5;
  --jp-cell-prompt-not-active-font-color: var(--md-grey-700);
  /* A custom blend of MD grey and blue 600
   * See https://meyerweb.com/eric/tools/color-blend/#546E7A:1E88E5:5:hex */
  --jp-cell-inprompt-font-color: #307fc1;
  /* A custom blend of MD grey and orange 600
   * https://meyerweb.com/eric/tools/color-blend/#546E7A:F4511E:5:hex */
  --jp-cell-outprompt-font-color: #bf5b3d;

  /* Notebook specific styles */

  --jp-notebook-padding: 10px;
  --jp-notebook-select-background: var(--jp-layout-color1);
  --jp-notebook-multiselected-color: var(--md-blue-50);

  /* The scroll padding is calculated to fill enough space at the bottom of the
  notebook to show one single-line cell (with appropriate padding) at the top
  when the notebook is scrolled all the way to the bottom. We also subtract one
  pixel so that no scrollbar appears if we have just one single-line cell in the
  notebook. This padding is to enable a 'scroll past end' feature in a notebook.
  */
  --jp-notebook-scroll-padding: calc(
    100% - var(--jp-code-font-size) * var(--jp-code-line-height) -
      var(--jp-code-padding) - var(--jp-cell-padding) - 1px
  );

  /* Rendermime styles */

  --jp-rendermime-error-background: #fdd;
  --jp-rendermime-table-row-background: var(--md-grey-100);
  --jp-rendermime-table-row-hover-background: var(--md-light-blue-50);

  /* Dialog specific styles */

  --jp-dialog-background: rgba(0, 0, 0, 0.25);

  /* Console specific styles */

  --jp-console-padding: 10px;

  /* Toolbar specific styles */

  --jp-toolbar-border-color: var(--jp-border-color1);
  --jp-toolbar-micro-height: 8px;
  --jp-toolbar-background: var(--jp-layout-color1);
  --jp-toolbar-box-shadow: 0px 0px 2px 0px rgba(0, 0, 0, 0.24);
  --jp-toolbar-header-margin: 4px 4px 0px 4px;
  --jp-toolbar-active-background: var(--md-grey-300);

  /* Statusbar specific styles */

  --jp-statusbar-height: 24px;

  /* Input field styles */

  --jp-input-box-shadow: inset 0 0 2px var(--md-blue-300);
  --jp-input-active-background: var(--jp-layout-color1);
  --jp-input-hover-background: var(--jp-layout-color1);
  --jp-input-background: var(--md-grey-100);
  --jp-input-border-color: var(--jp-border-color1);
  --jp-input-active-border-color: var(--jp-brand-color1);
  --jp-input-active-box-shadow-color: rgba(19, 124, 189, 0.3);

  /* General editor styles */

  --jp-editor-selected-background: #d9d9d9;
  --jp-editor-selected-focused-background: #d7d4f0;
  --jp-editor-cursor-color: var(--jp-ui-font-color0);

  /* Code mirror specific styles */

  --jp-mirror-editor-keyword-color: #008000;
  --jp-mirror-editor-atom-color: #88f;
  --jp-mirror-editor-number-color: #080;
  --jp-mirror-editor-def-color: #00f;
  --jp-mirror-editor-variable-color: var(--md-grey-900);
  --jp-mirror-editor-variable-2-color: #05a;
  --jp-mirror-editor-variable-3-color: #085;
  --jp-mirror-editor-punctuation-color: #05a;
  --jp-mirror-editor-property-color: #05a;
  --jp-mirror-editor-operator-color: #aa22ff;
  --jp-mirror-editor-comment-color: #408080;
  --jp-mirror-editor-string-color: #ba2121;
  --jp-mirror-editor-string-2-color: #708;
  --jp-mirror-editor-meta-color: #aa22ff;
  --jp-mirror-editor-qualifier-color: #555;
  --jp-mirror-editor-builtin-color: #008000;
  --jp-mirror-editor-bracket-color: #997;
  --jp-mirror-editor-tag-color: #170;
  --jp-mirror-editor-attribute-color: #00c;
  --jp-mirror-editor-header-color: blue;
  --jp-mirror-editor-quote-color: #090;
  --jp-mirror-editor-link-color: #00c;
  --jp-mirror-editor-error-color: #f00;
  --jp-mirror-editor-hr-color: #999;

  /* Vega extension styles */

  --jp-vega-background: white;

  /* Sidebar-related styles */

  --jp-sidebar-min-width: 250px;

  /* Search-related styles */

  --jp-search-toggle-off-opacity: 0.5;
  --jp-search-toggle-hover-opacity: 0.8;
  --jp-search-toggle-on-opacity: 1;
  --jp-search-selected-match-background-color: rgb(245, 200, 0);
  --jp-search-selected-match-color: black;
  --jp-search-unselected-match-background-color: var(
    --jp-inverse-layout-color0
  );
  --jp-search-unselected-match-color: var(--jp-ui-inverse-font-color0);

  /* Icon colors that work well with light or dark backgrounds */
  --jp-icon-contrast-color0: var(--md-purple-600);
  --jp-icon-contrast-color1: var(--md-green-600);
  --jp-icon-contrast-color2: var(--md-pink-600);
  --jp-icon-contrast-color3: var(--md-blue-600);
}
</style><style type="text/css">
/* Force rendering true colors when outputing to pdf */
* {
  -webkit-print-color-adjust: exact;
}

/* Misc */
a.anchor-link {
  display: none;
}

.highlight  {
  margin: 0.4em;
}

/* Input area styling */
.jp-InputArea {
  overflow: hidden;
}

.jp-InputArea-editor {
  overflow: hidden;
}

.CodeMirror pre {
  margin: 0;
  padding: 0;
}

/* Using table instead of flexbox so that we can use break-inside property */
/* CSS rules under this comment should not be required anymore after we move to the JupyterLab 4.0 CSS */


.jp-CodeCell.jp-mod-outputsScrolled .jp-OutputArea-prompt {
  min-width: calc(
    var(--jp-cell-prompt-width) - var(--jp-private-cell-scrolling-output-offset)
  );
}

.jp-OutputArea-child {
  display: table;
  width: 100%;
}

.jp-OutputPrompt {
  display: table-cell;
  vertical-align: top;
  min-width: var(--jp-cell-prompt-width);
}

body[data-format='mobile'] .jp-OutputPrompt {
  display: table-row;
}

.jp-OutputArea-output {
  display: table-cell;
  width: 100%;
}

body[data-format='mobile'] .jp-OutputArea-child .jp-OutputArea-output {
  display: table-row;
}

.jp-OutputArea-output.jp-OutputArea-executeResult {
  width: 100%;
}

/* Hiding the collapser by default */
.jp-Collapser {
  display: none;
}

@media print {
  .jp-Cell-inputWrapper,
  .jp-Cell-outputWrapper {
    display: block;
  }

  .jp-OutputArea-child {
    break-inside: avoid-page;
  }
}
</style> <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/latest.js?config=TeX-AMS_CHTML-full,Safe"> </script>  <script type="text/x-mathjax-config">
    init_mathjax = function() {
        if (window.MathJax) {
        // MathJax loaded
            MathJax.Hub.Config({
                TeX: {
                    equationNumbers: {
                    autoNumber: "AMS",
                    useLabelIds: true
                    }
                },
                tex2jax: {
                    inlineMath: [ ['$','$'], ["\\(","\\)"] ],
                    displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
                    processEscapes: true,
                    processEnvironments: true
                },
                displayAlign: 'center',
                CommonHTML: {
                    linebreaks: {
                    automatic: true
                    }
                }
            });

            MathJax.Hub.Queue(["Typeset", MathJax.Hub]);
        }
    }
    init_mathjax();
    </script> <div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [1]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="kn">import</span> <span class="nn">pandas</span> <span class="k">as</span> <span class="nn">pd</span>
<span class="kn">import</span> <span class="nn">matplotlib.pyplot</span> <span class="k">as</span> <span class="nn">plt</span>
<span class="kn">import</span> <span class="nn">dask.dataframe</span> <span class="k">as</span> <span class="nn">dd</span>
<span class="kn">import</span> <span class="nn">datetime</span> <span class="k">as</span> <span class="nn">dt</span>
<span class="kn">import</span> <span class="nn">seaborn</span> <span class="k">as</span> <span class="nn">sns</span>
```

</div> </div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [2]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="c1">#defining columns i will work with. this is important considering the original size of the dataset</span>
<span class="n">mycolumns</span> <span class="o">=</span> <span class="p">[</span><span class="s1">'REPORTER_CITY'</span><span class="p">,</span> <span class="s1">'REPORTER_STATE'</span><span class="p">,</span><span class="s1">'BUYER_CITY'</span><span class="p">,</span><span class="s1">'BUYER_STATE'</span><span class="p">,</span><span class="s1">'DRUG_NAME'</span><span class="p">,</span><span class="s1">'QUANTITY'</span><span class="p">,</span><span class="s1">'Ingredient_Name'</span><span class="p">,</span><span class="s1">'TRANSACTION_DATE'</span><span class="p">]</span>
```

</div> </div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [3]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="c1">#define the data type of each column to reduce calculation errors</span>
<span class="n">traintypes</span> <span class="o">=</span> <span class="p">{</span><span class="s1">'BUYER_CITY'</span><span class="p">:</span> <span class="s1">'str'</span><span class="p">,</span>
              <span class="s1">'BUYER_STATE'</span><span class="p">:</span> <span class="s1">'str'</span><span class="p">,</span>
              <span class="s1">'DRUG_NAME'</span><span class="p">:</span> <span class="s1">'str'</span><span class="p">,</span>
              <span class="s1">'QUANTITY'</span><span class="p">:</span> <span class="s1">'int64'</span><span class="p">,</span>
              <span class="s1">'TRANSACTION_DATE'</span><span class="p">:</span> <span class="s1">'int64'</span><span class="p">,</span>
              <span class="s1">'Ingredient_Name'</span><span class="p">:</span> <span class="s1">'str'</span>
              <span class="p">}</span>
```

</div> </div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [4]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="n">file_path</span> <span class="o">=</span> <span class="s1">'arcos_all_washpost.tsv'</span>
```

</div> </div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [5]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="n">pills</span> <span class="o">=</span> <span class="n">dd</span><span class="o">.</span><span class="n">read_csv</span><span class="p">(</span><span class="n">file_path</span><span class="p">,</span><span class="n">sep</span><span class="o">=</span><span class="s1">'</span><span class="se">\t</span><span class="s1">'</span><span class="p">,</span> <span class="n">usecols</span><span class="o">=</span><span class="n">mycolumns</span><span class="p">,</span> <span class="n">dtype</span><span class="o">=</span><span class="n">traintypes</span><span class="p">)</span>
<span class="n">pills</span><span class="o">.</span><span class="n">head</span><span class="p">(</span><span class="mi">5</span><span class="p">)</span>
```

</div> </div></div></div></div><div class="jp-Cell-outputWrapper"><div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser"></div><div class="jp-OutputArea jp-Cell-outputArea"><div class="jp-OutputArea-child"><div class="jp-OutputPrompt jp-OutputArea-prompt">Out[5]:</div><div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html"><div><style scoped="">
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>|  | REPORTER\_CITY | REPORTER\_STATE | BUYER\_CITY | BUYER\_STATE | DRUG\_NAME | QUANTITY | TRANSACTION\_DATE | Ingredient\_Name |
|---|---|---|---|---|---|---|---|---|
| 0 | BROCKTON | MA | MALDEN | MA | HYDROCODONE | 1 | 12262012 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE |
| 1 | PHOENIX | AZ | PHOENIX | AZ | HYDROCODONE | 4 | 3112009 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE |
| 2 | PHOENIX | AZ | GILBERT | AZ | HYDROCODONE | 40 | 11252008 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE |
| 3 | PHOENIX | AZ | GILBERT | AZ | HYDROCODONE | 20 | 6122009 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE |
| 4 | PHOENIX | AZ | GILBERT | AZ | HYDROCODONE | 10 | 10022009 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE |

</div></div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [6]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="c1">#graph of pills bought by state</span>

<span class="c1">#first we create the groupby, use compute() on that set, sort values, and then graph (Paul Mooney in Kaggle)</span>

<span class="c1">#def quantityCounter(dataframe,columnToCount):</span>
    <span class="c1">#quantityCount = dataframe.groupby(columnToCount)['QUANTITY'].sum()</span>
    <span class="c1">#quantityCount = quantityCount.compute().sort_values(ascending=False)</span>
    <span class="c1">#df = pd.DataFrame(data=quantityCount)</span>
    <span class="c1">#return df</span>
```

</div> </div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [7]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="n">state</span> <span class="o">=</span> <span class="n">pills</span><span class="o">.</span><span class="n">groupby</span><span class="p">(</span><span class="s1">'BUYER_STATE'</span><span class="p">)[</span><span class="s1">'QUANTITY'</span><span class="p">]</span><span class="o">.</span><span class="n">sum</span><span class="p">()</span>
<span class="n">state</span> <span class="o">=</span> <span class="n">state</span><span class="o">.</span><span class="n">compute</span><span class="p">()</span><span class="o">.</span><span class="n">sort_values</span><span class="p">(</span><span class="n">ascending</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
```

</div> </div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [8]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="n">state_df</span> <span class="o">=</span> <span class="n">state</span><span class="o">.</span><span class="n">reset_index</span><span class="p">()</span>
<span class="n">state_df</span><span class="o">.</span><span class="n">columns</span> <span class="o">=</span> <span class="p">[</span><span class="s1">'BUYER_STATE'</span><span class="p">,</span> <span class="s1">'QUANTITY'</span><span class="p">]</span>
```

</div> </div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [9]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="n">plt</span><span class="o">.</span><span class="n">figure</span><span class="p">(</span><span class="n">figsize</span><span class="o">=</span><span class="p">(</span><span class="mi">12</span><span class="p">,</span> <span class="mi">6</span><span class="p">))</span>  <span class="c1"># Adjust figure size as needed</span>
<span class="n">sns</span><span class="o">.</span><span class="n">barplot</span><span class="p">(</span><span class="n">data</span><span class="o">=</span><span class="n">state_df</span><span class="p">,</span> <span class="n">x</span><span class="o">=</span><span class="s1">'BUYER_STATE'</span><span class="p">,</span> <span class="n">y</span><span class="o">=</span><span class="s1">'QUANTITY'</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xticks</span><span class="p">(</span><span class="n">rotation</span><span class="o">=</span><span class="mi">90</span><span class="p">)</span>  <span class="c1"># Rotate x-axis labels for readability</span>
<span class="n">plt</span><span class="o">.</span><span class="n">ylim</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="nb">max</span><span class="p">(</span><span class="n">state_df</span><span class="p">[</span><span class="s1">'QUANTITY'</span><span class="p">])</span> <span class="o">*</span> <span class="mf">1.1</span><span class="p">)</span>

<span class="n">plt</span><span class="o">.</span><span class="n">tight_layout</span><span class="p">()</span>

<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s1">'Cantidad de Píldoras Compradas por Estado 2006-2012'</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
```

</div> </div></div></div></div><div class="jp-Cell-outputWrapper"><div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser"></div><div class="jp-OutputArea jp-Cell-outputArea"><div class="jp-OutputArea-child"><div class="jp-OutputPrompt jp-OutputArea-prompt"></div><div class="jp-RenderedImage jp-OutputArea-output ">![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABKUAAAJUCAYAAADEo5XNAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjcuMiwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy8pXeV/AAAACXBIWXMAAA9hAAAPYQGoP6dpAACAH0lEQVR4nOzdeZyN9f//8eeZHTP2fRtblkgRCoWxZS1bZcsytNhKJaGylOWjRYpIMYaSpUJRkexbGQkVsmTNlm0YzGC8f3/4zfk6s3DOnGuumcbjfrudG+da3ud1zlznOtf1vN7XdTmMMUYAAAAAAACAjXzSuwAAAAAAAADceQilAAAAAAAAYDtCKQAAAAAAANiOUAoAAAAAAAC2I5QCAAAAAACA7QilAAAAAAAAYDtCKQAAAAAAANiOUAoAAAAAAAC2I5QCAAAAAACA7QilAADIJN58803lz59f+/btS+9SAAAAgNsilAKAO9j27dvVvXt3lSxZUkFBQQoODlbVqlX19ttv68yZM2n2upcuXdLw4cO1atWqJOMiIyPlcDh04MCB27ZTr1491atXz9LaHA6Hhg8fnqp5V61aJYfDkez7So0DBw7I4XA4Hz4+PsqTJ4+aNWumjRs3uky7fPlyvfvuu1q8eLFKly7tUS3dunVTiRIlLKnZbtevX9dnn32mhg0bKm/evPL391f+/PnVokULLVq0SNevX0/vEtNFwrITGRmZ3qXYKmH9kdLDk+/m0aNHNXz4cG3dujXN6nRnPeeOFStWKDw8XOXLl1e2bNlUpEgRPfbYY/r111+TnX7Lli1q2LChgoODlTNnTrVp00Z///13stNOmDBB5cuXV2BgoEqWLKkRI0bo6tWryU77zTffqG7dusqePbuyZcumihUr6pNPPrlt/fHx8Ro3bpyaNGmiokWLKmvWrKpQoYIGDRqkc+fOpbquI0eOqH///qpbt65y5syZ4nfi/PnzGjVqlOrVq6eCBQsqODhY99xzj8aOHavY2Njb1g8ASD1CKQC4Q3366ae6//77FRUVpVdeeUVLlizRggUL9Pjjj+vjjz9Wjx490uy1L126pBEjRiS7g9i8eXNt3LhRhQoVSrPX/6/p16+fNm7cqLVr12rMmDHatm2bwsLC9Ntvv0mSjh07pi5duuiLL75QjRo10rla+8TGxqpZs2bq2rWr8ufPr8mTJ2vFihX6+OOPVbhwYT3++ONatGhRepeJdDB9+nRt3LgxyaNq1aput3H06FGNGDEiTUIpq02ePFkHDhzQCy+8oO+//14ffPCBTp48qQcffFArVqxwmXbXrl2qV6+erly5onnz5ikiIkK7d+/Www8/rH///ddl2lGjRumFF15QmzZttHTpUvXu3VujR49Wnz59ktTwv//9T23atFGlSpU0b948ffvtt+rdu7euXLly2/ovX76s4cOHKzQ0VOPHj9f333+vp59+Wp988olq166ty5cvp6quvXv3atasWQoICFCzZs1SfP1Dhw5p/Pjxqlq1qj755BN9++23ateunYYPH64WLVrIGHPb9wAASCUDALjjbNiwwfj6+pomTZqY2NjYJOPj4uLMN998k2av/++//xpJZtiwYV61U7duXVO3bl1LakrgTV0rV640kszKlSstqWX//v1GknnnnXdchi9fvtxIMj179rSklq5du5rQ0FAvq73h+vXr5tKlS5a0dTu9evUyksyMGTOSHb97926zbds2W2qxypUrV8zVq1e9bidh2Zk+fbr3RWUwFy9eTHHc9OnTjSQTFRXl9etERUWl2WeYUOf+/fstae/EiRNJhl24cMEUKFDANGjQwGX4448/bvLmzWuio6Odww4cOGD8/f3NwIEDncNOnTplgoKCzDPPPOMy/6hRo4zD4TB//vmnc9jmzZuNj4+PGTt2bKrqv3btmjl16lSS4V9++aWRZD777LNU1RUfH+/8/63+njExMSYmJibJ8HfeecdIMmvXrk3N2wIAuIGeUgBwBxo9erQcDoc++eQTBQYGJhkfEBCgRx991Pl87ty5aty4sQoVKqQsWbI4T6u4ePGiy3zdunVTcHCw9u7dq2bNmik4OFjFihXTyy+/rLi4OEk3TivKly+fJGnEiBHO02q6desmKfnTWowxevvttxUaGqqgoCBVrVpVP/zwQ5K6Y2Nj9fLLL+u+++5Tjhw5lDt3btWsWVPffPNNkmnPnz+vp59+Wnny5FFwcLCaNGmi3bt3u/0Z7tq1S02aNFHWrFmVN29ePffcc7pw4UKy0/70009q0KCBsmfPrqxZs6p27dpavny526+V2IMPPihJOnjwoHOYu6cdRkZGqly5cgoMDFSFChU0c+bMZKc7c+aMevfurSJFiiggIEClSpXSa6+95vw73vy6ffv21ccff6wKFSooMDBQM2bMkHTj7/vAAw8od+7cyp49u6pWrapp06Yl6XWwYsUK1atXT3ny5FGWLFlUvHhxtW3bVpcuXUrxfRw/flxTp07VI488oi5duiQ7zV133aXKlSs7nx86dEidO3dW/vz5ne//vffecznFL+G0t3feeUdjx45ViRIllCVLFtWrV0+7d+/W1atXNWjQIBUuXFg5cuRQ69atdfLkSZfXLVGihFq0aKEFCxaocuXKCgoKUqlSpfThhx+6TJdwiuVnn32ml19+WUWKFFFgYKD27t2rf//9V71799bdd9+t4OBg5c+fX/Xr19fatWuTvM+jR4/qiSeeUEhIiHLkyKEnn3xSx48fTzLd5s2b1b59e+d7KlGihDp06OCyHEk3ejIOGDDAeVpv7ty5Va1aNc2ePTvFv4f0f9/dZcuWqXv37sqdO7eyZcumli1bJntqWEREhO69917na7Ru3Vo7d+50mSZhnfL777+rcePGCgkJUYMGDW5Zh7u+/PJLPfDAA8qRI4eyZs2qUqVKKTw8XNKNv0316tUlSd27d3eupxK+Y+5+lpL0888/q3bt2goKClLhwoU1ePDgZE9/u379ut5++23nKWn58+dXly5ddOTIkdu+l/z58ycZFhwcrLvvvluHDx92Drt27ZoWL16stm3bKnv27M7hoaGhCgsL04IFC5zDlixZotjYWHXv3t2l3e7du8sYo4ULFzqHTZw4UYGBgerXr99ta02Or6+v8uTJk2R4Qs/Pm9+DJ3X5+Li3q5MtWzZly5bNrdcHAFjLL70LAADYKz4+XitWrND999+vYsWKuTXPnj171KxZM/Xv31/ZsmXTrl27NHbsWG3atCnJqSFXr17Vo48+qh49eujll1/WmjVr9NZbbylHjhwaOnSoChUqpCVLlqhJkybq0aOHevbsKUnOoCo5I0aM0IgRI9SjRw+1a9dOhw8f1tNPP634+HiVK1fOOV1cXJzOnDmjAQMGqEiRIrpy5Yp++ukntWnTRtOnT3eGF8YYtWrVShs2bNDQoUNVvXp1rV+/Xk2bNnXr8zhx4oTq1q0rf39/TZo0SQUKFNCsWbPUt2/fJNN+/vnn6tKlix577DHNmDFD/v7+mjJlih555BEtXbo0VTvYe/fulXTrzyw5kZGR6t69ux577DG99957io6O1vDhwxUXF+ey8xYbG6uwsDDt27dPI0aMUOXKlZ2nDm7dulXfffedS7sLFy7U2rVrNXToUBUsWNC5g3zgwAE9++yzKl68uKQbO+f9+vXTP//8o6FDhzqnad68uR5++GFFREQoZ86c+ueff7RkyRJduXJFWbNmTfa9rFy5UlevXlWrVq3ceu///vuvatWqpStXruitt95SiRIltHjxYg0YMED79u3TpEmTXKb/6KOPVLlyZX300Uc6d+6cXn75ZbVs2VIPPPCA/P39FRERoYMHD2rAgAHq2bOnvv32W5f5t27dqv79+2v48OEqWLCgZs2apRdeeEFXrlzRgAEDXKYdPHiwatasqY8//lg+Pj7Knz+/8zSqYcOGqWDBgoqJidGCBQtUr149LV++3HkttcuXL6thw4Y6evSoxowZo7Jly+q7777Tk08+meQzOHDggMqVK6f27dsrd+7cOnbsmCZPnqzq1atrx44dyps3ryTppZde0meffaaRI0eqSpUqunjxov744w+dPn3arc+6R48eatSokb744gsdPnxYr7/+uurVq6ft27crZ86ckqQxY8ZoyJAh6tChg8aMGaPTp09r+PDhqlmzpqKionTXXXc527ty5YoeffRRPfvssxo0aJCuXbt22xri4+OTTOdwOOTr6ytJ2rhxo5588kk9+eSTGj58uIKCgnTw4EHn+qxq1aqaPn26unfvrtdff13NmzeXJBUtWtSjz3LHjh1q0KCBSpQoocjISGXNmlWTJk3SF198kaTmXr166ZNPPlHfvn3VokULHThwQG+88YZWrVqlLVu2ONt0V3R0tLZs2aL69es7h+3bt0+XL192CWsTVK5cWcuWLVNsbKyCgoL0xx9/SJLuuecel+kKFSqkvHnzOsdL0po1a1ShQgV9/fXXeuutt7R3714VKlRInTt31ptvvqmAgACPak+Q8PeoWLGic5gndXkrudcHAFgsXftpAQBsd/z4cSPJtG/fPlXzX79+3Vy9etWsXr3aSHI5Papr165Gkpk3b57LPM2aNTPlypVzPr/V6XuJT2s5e/asCQoKMq1bt3aZbv369UbSLU/fu3btmrl69arp0aOHqVKlinP4Dz/8YCSZDz74wGX6UaNGuXX63quvvmocDofZunWry/BGjRq5nDJ38eJFkzt3btOyZUuX6eLj4829995ratSoccvXSTgFa+zYsebq1asmNjbW/Prrr6Z69epGkvnuu++c0yauO/Hpe/Hx8aZw4cKmatWq5vr1687pEk7bufn0vY8//jjZv+PYsWONJPPjjz+6vG6OHDnMmTNnbvle4uPjzdWrV82bb75p8uTJ46zhq6++MpKSfJa387///c9IMkuWLHFr+kGDBhlJ5pdffnEZ3qtXL+NwOMxff/1ljPm/z/zee+91OfVn/PjxRpJ59NFHXebv37+/keRyKlRoaGiKy0f27Nmdp58l/I3q1Klz2/oTluUGDRq4fBcmT55sJCU53fbpp5++7aln165dMzExMSZbtmwu34VKlSqZVq1a3bamxBK+uyl9V0eOHGmMufGdzpIli2nWrJnLdIcOHTKBgYGmY8eOzmEJ65SIiAiPakju4evr65zu3XffNZLMuXPnUmzLk9P3Uvosn3zySZMlSxZz/Phxl2nLly/vsp7buXOnkWR69+7t0u4vv/xiJJkhQ4a49f5v1qlTJ+Pn52c2b97sHJbwt5g9e3aS6UePHm0kmaNHjxpjbixDgYGBybZdtmxZ07hxY+fzwMBAExISYnLlymUmTpxoVqxYYV577TXj6+vr8vf0xJEjR0yBAgVMtWrVXL6LntR1M09Px9y2bZvJkiVLkuUZAGCtTHP63po1a9SyZUsVLlxYDofDpeuuO4YPH57sXVqS68oLAHeav//+Wx07dlTBggXl6+srf39/1a1bV5KSnG7jcDjUsmVLl2GVK1dO9rQWd2zcuFGxsbHq1KmTy/BatWopNDQ0yfRffvmlateureDgYPn5+cnf31/Tpk1zqXPlypWSlKTNjh07ulXTypUrVbFiRd177723nH/Dhg06c+aMunbtqmvXrjkf169fV5MmTRQVFZXkFMjkvPrqq/L391dQUJDuv/9+HTp0SFOmTLnlhXsT++uvv3T06FF17NhRDofDOTw0NFS1atVymXbFihXKli2b2rVr5zI84RTLxKce1q9fX7ly5UrymitWrFDDhg2VI0cO53IzdOhQnT592nnK23333aeAgAA988wzmjFjRop3APPWihUrdPfddye5EHy3bt1kjEnS469Zs2YuvccqVKggSc4eM4mHHzp0yGV4SsvH+fPntWXLFpfhbdu2Tbbmjz/+WFWrVlVQUJBzWV6+fHmSZTkkJMTldNuE10osJiZGr776qsqUKSM/Pz/5+fkpODhYFy9edGmzRo0a+uGH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6ty5c/rggw9crpeX0Kto1qxZqlChgoKDg1W4cGEVLlzY7c/y9ddf17fffqv69etr6NChypo1qz766KMk15IrV66cnnnmGU2YMEE+Pj5q2rSp8+57xYoV04svvnjL9/vee+9p6NChatKkiZo3b66ff/7ZZfzNPTVHjBih6tWrq0WLFho0aJBiY2M1dOhQ5c2bVy+//LJzuty5c+v111/XG2+8ody5c6tx48aKiorS8OHD1bNnT919993Oabt3764pU6aod+/eOnXqlO6++2799NNP+uijj9S7d+/bBjyXL1/WI488ot9++03jx4/XtWvXXN5Dvnz5VLp0aY/rkqSvvvpKkpzXq9u8ebPz7IiE6+adPHlSYWFhOnbsmKZNm6aTJ086r3sn3eitSq8pAEgj6XWF9bQkySxYsMD5fN68eUaSyZYtm8vDz8/PPPHEE0nm/+KLL4yfn585duyYjVUDgP22bt1qunbtaooXL24CAgJMtmzZTJUqVczQoUPNyZMnndNt2LDB1KxZ02TNmtXky5fP9OzZ02zZsiXJnYy6du1qsmXLluR1hg0bZhL/5Pz000+mSpUqJjAw0EgyXbt2NcYkvfueMTfu+DdmzBhTrFgxExAQYCpXrmwWLVpk6tatm+Tue//73/9MiRIlTGBgoKlQoYL59NNPk339c+fOmfDwcJMzZ06TNWtW06hRI7Nr1y637r5njDE7duwwjRo1MkFBQSZ37tymR48e5ptvvnG5412C1atXm+bNm5vcuXMbf39/U6RIEdO8eXPz5Zdf3vI1Eu4E984779y2nsR1J777XoKpU6eau+66ywQEBJiyZcuaiIgI07VrV5e77xljzOnTp81zzz1nChUqZPz8/ExoaKgZPHiwiY2NTfK6ffr0SbamiIgIU65cORMYGGhKlSplxowZY6ZNm+by9924caNp3bq1CQ0NNYGBgSZPnjymbt265ttvv73tezbmxp3MZsyYYerXr29y585t/Pz8TL58+UzTpk3NF1984XLXroMHD5qOHTuaPHnyGH9/f1OuXDnzzjvvuEyT0mee8Hkm/pslLK9RUVHOYaGhoaZ58+bmq6++MhUrVjQBAQGmRIkSZty4cW61aYwxcXFxZsCAAaZIkSImKCjIVK1a1SxcuDDZv9WRI0dM27ZtTXBwsAkJCTFt27Y1GzZsSPL9TJguV65cJiQk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AAAAtiOUAgAAAAAAgO0IpQAAAAAAAGA7Qikb7dix47bTfP755zZUAgDAf8+1a9fSuwQAQCZ06NAhWXn/r+vXr1vWFpDZ3ZGhVHpt1N5///169913k13hnThxQo8++qh69eqVDpXdecLDw2/76NGjh+WveyftUG3dujW9S7DE2rVrdeXKlRTHx8bGaubMmZa81tmzZzVhwgTdd999lrSHjOnSpUuWtHPt2jUdOnTIkrYygjlz5txy/NWrV9W2bVubqrmzWf1bNXPmTMXFxVnaJoA72z///HPbaWbNmuV2eyVLltS///7rTUkuatasqd27d1vWHpCZ3VGh1I4dO/TSSy+pSJEilrV5+PBhhYeHuzXt559/rrffflt16tTRvn37XIbffffdio6O9nhHfsKECR5N742MsMN8+fJlffvtt7pw4UKScefPn9e3337r1obv2bNnU3ycOnVKc+bMUWRkpGV1p8Wy56k9e/aoQ4cOOn/+fJJx0dHR6tixo/7++2+vXiM6OlqTJk1S1apVdf/993vVVkZRt25d1alTR8eOHUt2fHR0tLp37+7Va/z000/q0KGDChcurLffflt169b1qr3EPFlPSVLbtm11+vRpS2u42alTp9K0fSljhqKxsbF67733VKpUKUva+/PPP1WyZEm3p//www/delgpPj5eCxcudGvabt26aenSpSm28/jjj2vz5s0e1/Dll1+qTZs2qlSpku655x61adNGX331lcftSFL//v31xx9/pGped127dk0xMTGWtnns2DH17dvX7ekLFSqkAQMGaOfOnZa8fvfu3RUdHW1JW2mlVKlSab5e+i/JiOvQzOrkyZO3HH/t2jVt2rTJrbbCw8OT3UbOjBo1aqSzZ8+mOP6LL77waPvMyl5SkhQaGqoqVapYvq+2Z88evfvuu+rbt6/69euncePGeb39furUKW3evFm//vprplwPbt++3a2Hu8LCwlS/fv0kj9atW2vQoEE6fPhwGr4be23atEnx8fHO54m/J3FxcZo3b573L2QyuQsXLphPP/3UPPjgg8bX19fUrl3bjBs3zrL2t27danx8fNye/sSJE6ZVq1YmW7Zs5p133jGPPvqoyZo1qxk/fry5fv26x6+fK1cu07BhQ3P48GGP53XXsmXLTPv27U1QUJApWrSoef75592aL2fOnCZXrly3fXhi/Pjx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WjWVL1/euaxY5ejRoxo2bJhmzJihRx55RFu3bvX4s/Tx8ZHD4bjlNA6HQ9euXVOZMmX02WefudVunTp1FBUVpdatW6t69eqaPHmyR3V98skn+uCDDzRv3jxFRESof//+euSRR2SMSfXtg61cTyU4ceKEevbsqXXr1mnatGnq2rVrqmqTblxk+Vbfy0qVKun48eNut/fjjz/q+eefV69evXTXXXeluq4E27dv19tvv53i+KZNm7p9AfCrV68qICDA+dzf3185cuRIVV333XefHA7HLb//t1vGb/bwww/r4Ycf1uuvv66rV69q48aNWrVqlVatWqXZs2crLi5OZcqU0V9//eV2m3v37lX79u0VFhamChUquD1fcl566SXn/3PmzCljjO67774kN50YN26cW+1lyZJF586dU/HixZMdf/78eWXJkiXV9Sa4ePGiVq9erdWrV+vcuXMqV66cW/MVLlxYO3fuVPHixRUTE6Nt27bp/fffd44/ffq0smbN6lZb58+fV86cOSVJfn5+ypIli8qWLevxe0nJpUuXNHfuXE2fPl3r169X6dKl9corr3jUxocffqj8+fNbVtPNrPjNkKRffvlF+fLls6SmAgUKaP/+/SpWrJiuXLmiLVu2aMSIEc7xFy5ckL+/v1ttVa9eXdWrV9f777+vr776StOnT9err76qli1bqkePHmrUqJFHtVm9DnU4HEnWRZ6smxKzcp38X+BwOHThwgUFBQXJGCOHw6GYmBjnjWmSu0HNrYwYMSLVvzuJnTt3Tn369NGPP/4of39/DRo0SH379tXw4cP17rvvqmLFioqIiHC7vZw5c7q1bNx8YeWU3Hxh6nfeeUddunRR69at1bJlS5dxlStXdrs+K40ZM0YjRoxQx44d9cEHHygkJMSr9k6cOKESJUqkOL5kyZIebUv9/fffqlq1aorjq1Wr5tEF1Hfu3KkZM2YoODhYzz//vAYOHKjx48erTp06brdxsxw5cmjfvn0KDQ1NdvzevXuVPXt2t9qqWbOmatasqQ8//FDz5s3T9OnT1bBhQ5UoUULh4eHq2rWrihYtmqo6k/Pjjz96/BscHx/vsv0o3fg9t+JuuLGxsVq/fr1WrVqllStXKioqSqGhoXriiSfcmn/Hjh3OZcsYo127djlvyHLq1Cmv65OkTBdKDRkyREOGDNHq1asVERGhBx98UKVLl5Yx5pZ3aEjJ7VbqOXLkUJcuXdxqa9OmTSmuGBs1aqTff/9dL774okf1/f333+rSpYv27dunL774Qo899phH8yeWFjvMVgoPD9dLL72kihUrqkWLFi7jFi1apJEjR7q1wxIbGytJt9zocTgcbv0oJrB62Xvuuec0Z84c/f333woPD1fnzp2VO3duj9tJMHjwYNWsWVPt2rXTwIEDnTtOu3bt0ttvv62lS5dqw4YNbrWVsHIfP3685s6dq4iICL300ku6fv26li1bpmLFiqXqxzc6OlqjR4923uVx+fLlbgdliS1YsCDFcRs2bNCECROcO/sBAQEefXeKFy+u9evXq0ePHkmWw9vZunWr7rvvPnXt2lVdu3bVnj17NG3aNG3evFm1a9dW8+bN1a5du9sGTTfLnj37LTfyPFlPSdLs2bPVr18/ValSRdu3b1exYsWSTHP+/Hm3Nwby5s2rAwcOpPiDv3//fuXJk8ft+qwORc+cOaMCBQqkOL5AgQIefYeHDh3qDBOuXLmikSNHJvktcWc9tX///ttOk5p1i3QjLKtTp46qV6+umjVraunSpfr000+1d+9ej9rZv3+/IiMj1atXL12+fFkdOnRQp06dUrVD+ttvv7k8r1mzpq5du+Yy3JN2a9asqcmTJ6cYHH/00UeqWbOmx3UmWLNmjaZPn+68k9/jjz+usWPHqnbt2m7N365dO/Xv319DhgzR999/r4IFC+rBBx90jt+8ebPbAZeUdKPxr7/+0sWLF12m8XTnbO3atc73GB8fr3bt2mnkyJEe72R4E1DcipW/GdKNdbtVwVmTJk00aNAgjR07VgsXLlTWrFldatu+fbtKly7tUZtBQUHq3LmzOnfurP3796tHjx5q0qSJ/v33X4+2D6xehxpj1K1bNwUGBkq6sY313HPPKVu2bJLk1l2Rb2b1OlmSpk6dquDgYEk37mYXGRmpvHnzSlK6363OGOOyA2uMUZUqVVyee/Idat++vWXL8ZAhQ7RmzRp17dpVS5Ys0YsvvqglS5YoNjZWP/zwg8d3C165cqXz/8YYNWvWTFOnTk3V3alvPnCT8O+8efP05ZdfOrfvPNmWTy5c9cZ7772nr776yuPtxJTExsYmCS1u5u/vrytXrrjd3oULF265HRcSEuLRnWCtPjhi5cH0BFmyZHFuf+/bt0/Tp0/XlClTNHz4cDVq1Ejff/+9W+18++23yQ6Pjo5WVFSUpk2b5vFd3BOvR6Wk61JJmj9/vlvtrVy50vmIiopSqVKlVLduXfXt21d169ZVoUKF3K6tQYMGLgdIEy/TVnxvHMaKLhgZ2IULFzRr1ixNnz5dv/76q2rUqKF27dq5HJG9lb///lslSpRw9mzyRnh4uCVJ+c2Cg4P1yCOPaMqUKc4fVyvt3btXU6dO1WeffaaYmBiPdpgTf8YfffSROnfunKqds5t17txZX3zxhcqXL+/s8bRz507t3r1bTzzxhGbPnu1Re2nl/Pnz+uKLLxQREaFff/1VDzzwgEfLnnRjQ27+/PmKiIjQhg0b1Lx5c/Xo0UONGzdO1Qpg8eLFCg8Pd7ndqzFGefPm1dSpU93uGbZ3716VKVPGZdhff/2ladOm6bPPPtO5c+fUqFGjFFfayXn77bc1duxYFSxYUKNHj/Y6YE3Orl27NHjwYC1atEidOnXSW2+9lWIvigRhYWFasGCB84c2sXfffVevvvqq2xs9Pj4+qlKlinr27KlOnTo5NwiuX7+u7777TtOmTdMPP/zg8Ua8lYKDg/W///0vxVvInz9/Xo0bN9bPP//sVns9evTQ3r17tWzZsiQbVHFxcXrkkUdUunRpTZs2zaM6L126pDlz5igiIsJ5y9px48YpPDzco/Wsr6+vjh8/nmIPiRMnTqhw4cJu/Y3r1avnVg+9FStWuF1fYtHR0Zo1a5amTZumrVu3ehSex8bGasOGDVq5cqVWrVqlqKgolSxZUnXr1lWdOnVUt27dVO0cSNKKFSsUERGh+fPnKzY2VgMGDFDPnj1TvVF66tQpORwOjwLLm23YsEH16tVTq1atNGDAAJUvX17GGO3cuVPvvfeevvnmG61cudLtEEmSjhw5ohkzZigyMlL79u3TAw88oB49eqh9+/bOHV53Xbp0Sc8++6wWL16sggUL6pNPPnHZwA4LC1OTJk306quv3rathJ6ht+tV5+6yMnr0aEVGRmrv3r2qXr26wsPD1aFDB7eD6OTqO3HihGW9kCTrfzN8fHx0/Phxy3bm//33X7Vp00br169XcHCwIiMjXbadGjRooAcffFCjRo3yqN0jR44oMjJSkZGRunz5sp566imNHDkyVT3krVqHduvWza1tkunTp7vVnpXrZEkqUaKEW/W5cyBAStpzqWjRolq3bl2SXizufl9Wr17t1nTuBEC+vr46duyYZctxaGiopk2bpoYNG+rvv/9WmTJl9Pzzz2v8+PGWtB8SEqJt27apVKlSHs978ODB205z9uxZ3XfffW61d/P2WceOHb3ubebj46MiRYooLCzM+bhVTyd32hs5cmSKvzUXLlzQ0KFD3f5e+Pr6avfu3bf8npUvX96j7dsVK1Y4A/JatWpp3rx5SQ5Iuntw5LffflPNmjXVokWLZA+mf/fdd9qwYcMte3vdTkxMjGbNmqUhQ4bo3LlzHr3X5ISEhKh8+fIaMGCAHn/8cY9qsXo96uPjo+LFi2vQoEFq27Ztqn9/rf6epSTTh1I3++OPPzRt2jTNmjVLJ0+edGuexCv3J598Uh9++OEtj+C425YVPv/8c3Xu3Nmy9i5duqRXXnlFCxcu1NWrV9WwYUN9+OGHyp07t8c7zGFhYbedJrU7Z/PmzdOsWbO0d+9e5xGmjh07ut0NMS0CwltJzbKX2MGDBxUZGamZM2fq6tWr2rFjh8c7QZJ0+fJlLVmyxPnZlStXTo0bN/boNJabf2jr16+vsLAwZ/fa+Ph4LVq0SBERER6FUj4+PsqSJYsaNmwoX1/fFKdz9wjBzRKf2jFmzBivTq2U/i8YmDp1qrZt2+b2D9nGjRsVERGhefPm6erVq2rTpo169Ojh8n05efKkR+sJd0Jih8Ohr7/+2q32fH19NXXqVHXv3j3JuJiYGDVq1EjR0dHasWOHW+0dOXJE1apVU2BgoPr06aPy5ctLutGzY9KkSYqLi9PmzZuT7ZHlLm9CUR8fHzVt2tTl6NTN4uLitGTJEo/CnwTeBis3uzn0CQ0NVdu2bdW2bVuXI+q3UrduXUVFRal06dLOAKpu3bqp+j27lYTvRkREhLZs2aJKlSq5nEpxK+fOndNrr72muXPnOntC5MqVS+3bt9fIkSNTDIdTsmDBAj3zzDM6c+aMy/BcuXJpypQpatu2rUft+fn5KU+ePHrqqafUo0cPr05ZfPfddzVgwIAUx3sS/lq90ZgnTx517dpV4eHhXq8rpRvL7jPPPKMtW7Yk2VGPjo5WrVq19PHHH3t01Nvq34z69etrwYIFlp32lCA6OlrBwcFJajxz5oxCQkLcOoXvypUrWrBggaZNm6a1a9eqadOmCg8PV7NmzSw5aCp5f2DJSmm5TrZC4ssDJO7JlPA8PeqzOgD29/fXwYMHVbhwYUlS1qxZtWnTJkvWC5J3oVRKUnvg5ueff9a0adNuuX3miXXr1jlPkd+4caNiY2NVvHhx53ZzWFiYRweBrA5Xb3eZC0+X41uti27u0ebJ98Kqg+mJJZzV8vXXX8vX11dPPPGEevTo4dJb+b/u1Vdf1erVq/Xbb7+pXLlyqlu3rurVq6c6depYsn7w5gBpcjJdKLVixQr17dtXP//8c7IbPgnnkzZs2NCt9hIfOfNm5Wn1UbiENt29bo47XnnlFU2aNEmdOnVSUFCQZs+erXr16unLL790TuPpDnMCK3fOvJUWAeHly5e1fPlyZ5fGwYMHu4R3vr6+Gjp0aKqDsEOHDjmPjl65ckW7du3yKJT65ZdfdObMGTVt2tQ5bMaMGRo+fLguXryoVq1aacKECSluBN5s7dq1Wr16dbI/tPXr11e9evU87m1h9RECKempHWPHjvXq1A7J+2AgweXLl53nta9du9ar89qTC4+S4+5n99VXX+mpp57S7Nmz1apVK+fwmJgYNW7cWKdPn9aaNWs8CjP+/vtv53Upbu5W36hRI02cODFJz7vUio+P1+LFixUREaFvvvnGrXmsXvasDFYSekVERETo4sWLeuKJJ/Txxx9r27Ztuvvuu91uR7qxc1GoUCG1atXKuWGSFj1sb7ZmzRpnr6TbOXPmjGrWrKl//vlHnTp1UoUKFZw9m7744gsVK1ZMGzZs8Pg6h5cuXdLSpUud1w0sW7asGjdu7Pb1mm42f/58PfbYY7cMQdyVJUsWTZo0KcXwt3Hjxjp37pzb4W9yUrvRmPjAQ7169bw6wv/YY4+pXr16KV6i4MMPP9TKlStvedp1YmlxVDm59rJnz65y5cpp4MCBHp1WHR4e7tZ07lyPJ0+ePAoJCVHXrl311FNPpbjtktqebDdLzTrUnffqcDjc7g2bFtsD169fV2RkpObPn68DBw7I4XCoVKlSatu2rZ566imPep+vWrXKrendPbXNym357t27u1Wbu9eBStxrLSQkRNu3b1fJkiXdmv92rAylMuL2WYLE13H8+eefU3UdRytZ2UNPcu/giKQUrxGVksuXLzt/wxM6IqTmN/zw4cPO/aj9+/erVq1a6tGjh5544gmX0+PcERsbq59++inFfT4/Pz+9+eabCgoKcrtNq9ejCWJiYrR27Vrnsvfbb7+pbNmyqlu3rsLCwtSuXTuP2rPqe5ZYpgulHn30UYWFhVm24WN1KGV19/VbbTDcfN2cy5cvu9Ve6dKlNWrUKLVv317Sjetg1a5dW7GxsanaCLf6qLdVP9xpERBOmTJFixcv1qJFiyTdWFYqVqzo7IH0119/6ZVXXvHoumE3n763bt06tWjRQt27d1eTJk08PjratGlT1atXz3kqyO+//677779fXbt2VYUKFfTOO+/o2Wef1fDhwz1qNyP+0ErWntphZTCQnITz2mfOnKljx455dF57Wpk6daqef/55fffddwoLC1NMTIyaNGmikydPavXq1R6di36zs2fPOsOBMmXKpOo6aVbu7FnNymClWbNmzu99p06d1KRJE/n6+srf3z9Vy97FixedGyYrV67U1q1bnRsm9erVU926dS39fZKkbdu2qWrVqm6FIf3799fy5cv1008/JQk8jx8/rsaNG6tBgwYuFwO/ldsdpEpt7xyrdh6/+uorde7cWXPmzEkx/F29erUKFizodn0JvN1otPoIf/HixbV06dIUe5bt2rVLjRs31qFDh9xu02opbU+dO3dOmzZt0vTp0zVjxgy3T8nw8fFx62Yl7myP3vx7n9zy52kPBHfDNXd7mVn5XtOCMUYtWrTQDz/8oHvvvdflVN7ff/9djz76qBYuXJgutUnWbstb/bdI3Gtt0aJFql+/fpKd+NT0Ype8D7n+a9tnly9f1rp165zXcYyJiXH7e5vcweWZM2dq2LBhHh9cTguXL1/WgAEDkpxt483Br+TC5JIlS6pdu3YehcmNGjXSypUrlS9fPnXp0kXh4eEeXbMxsdvt8+3atUsDBw70aJ/PrvXomTNnNG7cOE2YMMHt5S+tv2dSJgylQkNDtWTJEss2fKw8QuDj46McOXLc9guU+DQDT6XmujkJAgICtH//fpeNzSxZsmj37t0en1qTFke9rfrhTouAsE6dOnrxxRfVunVrSUkDzM8//1wfffSRNm7c6FZ7vXv31pw5c1S8eHF1795dnTt39qqXWaFChbRo0SJVq1ZNkvTaa69p9erVWrdunSTpyy+/1LBhw1J9VN6bH1rJ+lPQrDq1w+pgICWpPa89Lb399tsaNWqUvvnmG73xxhs6duyYVq9e7XEvOKtDJHd+uB0Oh9sbyVYenbIyWPHz80v2DllWLXsXLlzQunXrnNeX2rZtm+66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)</div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [10]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="c1">#So... the initial plot I have created, vertical, didnt run properly after restarting the kernel.</span>
<span class="c1"># I had to check again with ai to rebuild it. </span>


<span class="c1">#plt.figure(figsize=(8,14))</span>
<span class="c1">#ax = </span>
<span class="c1">#sns.barplot(data=state_df, x='BUYER_STATE', y='QUANTITY', orient='v', hue='BUYER_STATE')</span>
<span class="c1"># Add data labels </span>

<span class="c1">#(asked Copilot, ChatGPT and Perplexity for a suggestion; chose Perplexity as it allowed to see the </span>
<span class="c1">#absolute value and not exponential notation (which i should investigate more on how to fix it from the start)</span>

<span class="c1">#for i, v in enumerate(state_df['QUANTITY']):</span>
    <span class="c1">#ax.text(v, i, f' {v:,.0f}', va='center', ha='left')</span>

<span class="c1"># Adjust x-axis to make room for labels</span>
<span class="c1">#plt.xlim(0, max(state_df['QUANTITY']) * 1.1)</span>

<span class="c1">#plt.tight_layout()</span>

<span class="c1">#plt.title('Cantidad de Píldoras Compradas por Estado')</span>
```

</div> </div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [11]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="c1">#converting the data column to a datetime type, first testing with a sample of the set</span>

<span class="n">fecha</span> <span class="o">=</span> <span class="n">pills</span><span class="o">.</span><span class="n">head</span><span class="p">()</span>
```

</div> </div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [12]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="n">fecha</span>
```

</div> </div></div></div></div><div class="jp-Cell-outputWrapper"><div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser"></div><div class="jp-OutputArea jp-Cell-outputArea"><div class="jp-OutputArea-child"><div class="jp-OutputPrompt jp-OutputArea-prompt">Out[12]:</div><div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html"><div><style scoped="">
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>|  | REPORTER\_CITY | REPORTER\_STATE | BUYER\_CITY | BUYER\_STATE | DRUG\_NAME | QUANTITY | TRANSACTION\_DATE | Ingredient\_Name |
|---|---|---|---|---|---|---|---|---|
| 0 | BROCKTON | MA | MALDEN | MA | HYDROCODONE | 1 | 12262012 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE |
| 1 | PHOENIX | AZ | PHOENIX | AZ | HYDROCODONE | 4 | 3112009 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE |
| 2 | PHOENIX | AZ | GILBERT | AZ | HYDROCODONE | 40 | 11252008 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE |
| 3 | PHOENIX | AZ | GILBERT | AZ | HYDROCODONE | 20 | 6122009 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE |
| 4 | PHOENIX | AZ | GILBERT | AZ | HYDROCODONE | 10 | 10022009 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE |

</div></div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [13]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="n">fecha</span><span class="p">[</span><span class="s1">'DATUM'</span><span class="p">]</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">to_datetime</span><span class="p">(</span><span class="n">fecha</span><span class="p">[</span><span class="s1">'TRANSACTION_DATE'</span><span class="p">]</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="nb">str</span><span class="p">),</span> <span class="nb">format</span><span class="o">=</span><span class="s1">'%m</span><span class="si">%d</span><span class="s1">%Y'</span><span class="p">)</span>
```

</div> </div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [14]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="n">fecha</span>
```

</div> </div></div></div></div><div class="jp-Cell-outputWrapper"><div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser"></div><div class="jp-OutputArea jp-Cell-outputArea"><div class="jp-OutputArea-child"><div class="jp-OutputPrompt jp-OutputArea-prompt">Out[14]:</div><div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html"><div><style scoped="">
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>|  | REPORTER\_CITY | REPORTER\_STATE | BUYER\_CITY | BUYER\_STATE | DRUG\_NAME | QUANTITY | TRANSACTION\_DATE | Ingredient\_Name | DATUM |
|---|---|---|---|---|---|---|---|---|---|
| 0 | BROCKTON | MA | MALDEN | MA | HYDROCODONE | 1 | 12262012 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE | 2012-12-26 |
| 1 | PHOENIX | AZ | PHOENIX | AZ | HYDROCODONE | 4 | 3112009 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE | 2009-03-11 |
| 2 | PHOENIX | AZ | GILBERT | AZ | HYDROCODONE | 40 | 11252008 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE | 2008-11-25 |
| 3 | PHOENIX | AZ | GILBERT | AZ | HYDROCODONE | 20 | 6122009 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE | 2009-06-12 |
| 4 | PHOENIX | AZ | GILBERT | AZ | HYDROCODONE | 10 | 10022009 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE | 2009-10-02 |

</div></div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [15]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="n">pills</span><span class="p">[</span><span class="s1">'DATUM'</span><span class="p">]</span> <span class="o">=</span> <span class="n">dd</span><span class="o">.</span><span class="n">to_datetime</span><span class="p">(</span><span class="n">pills</span><span class="p">[</span><span class="s1">'TRANSACTION_DATE'</span><span class="p">]</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="nb">str</span><span class="p">),</span> <span class="nb">format</span><span class="o">=</span><span class="s1">'%m</span><span class="si">%d</span><span class="s1">%Y'</span><span class="p">)</span>
```

</div> </div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [16]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="n">pills</span><span class="o">.</span><span class="n">head</span><span class="p">()</span>
```

</div> </div></div></div></div><div class="jp-Cell-outputWrapper"><div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser"></div><div class="jp-OutputArea jp-Cell-outputArea"><div class="jp-OutputArea-child"><div class="jp-OutputPrompt jp-OutputArea-prompt">Out[16]:</div><div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html"><div><style scoped="">
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>|  | REPORTER\_CITY | REPORTER\_STATE | BUYER\_CITY | BUYER\_STATE | DRUG\_NAME | QUANTITY | TRANSACTION\_DATE | Ingredient\_Name | DATUM |
|---|---|---|---|---|---|---|---|---|---|
| 0 | BROCKTON | MA | MALDEN | MA | HYDROCODONE | 1 | 12262012 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE | 2012-12-26 |
| 1 | PHOENIX | AZ | PHOENIX | AZ | HYDROCODONE | 4 | 3112009 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE | 2009-03-11 |
| 2 | PHOENIX | AZ | GILBERT | AZ | HYDROCODONE | 40 | 11252008 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE | 2008-11-25 |
| 3 | PHOENIX | AZ | GILBERT | AZ | HYDROCODONE | 20 | 6122009 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE | 2009-06-12 |
| 4 | PHOENIX | AZ | GILBERT | AZ | HYDROCODONE | 10 | 10022009 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE | 2009-10-02 |

</div></div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [17]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="c1">#Checking usage of drugs across time, but I should work with the years and months maybe separately</span>
<span class="n">pills</span><span class="p">[</span><span class="s1">'year'</span><span class="p">]</span> <span class="o">=</span> <span class="n">pills</span><span class="p">[</span><span class="s1">'DATUM'</span><span class="p">]</span><span class="o">.</span><span class="n">dt</span><span class="o">.</span><span class="n">year</span>
<span class="n">pills</span><span class="p">[</span><span class="s1">'month'</span><span class="p">]</span> <span class="o">=</span> <span class="n">pills</span><span class="p">[</span><span class="s1">'DATUM'</span><span class="p">]</span><span class="o">.</span><span class="n">dt</span><span class="o">.</span><span class="n">month</span>
```

</div> </div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [18]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="n">pills</span><span class="o">.</span><span class="n">head</span><span class="p">()</span>
```

</div> </div></div></div></div><div class="jp-Cell-outputWrapper"><div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser"></div><div class="jp-OutputArea jp-Cell-outputArea"><div class="jp-OutputArea-child"><div class="jp-OutputPrompt jp-OutputArea-prompt">Out[18]:</div><div class="jp-RenderedHTMLCommon jp-RenderedHTML jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/html"><div><style scoped="">
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>|  | REPORTER\_CITY | REPORTER\_STATE | BUYER\_CITY | BUYER\_STATE | DRUG\_NAME | QUANTITY | TRANSACTION\_DATE | Ingredient\_Name | DATUM | year | month |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 0 | BROCKTON | MA | MALDEN | MA | HYDROCODONE | 1 | 12262012 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE | 2012-12-26 | 2012 | 12 |
| 1 | PHOENIX | AZ | PHOENIX | AZ | HYDROCODONE | 4 | 3112009 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE | 2009-03-11 | 2009 | 3 |
| 2 | PHOENIX | AZ | GILBERT | AZ | HYDROCODONE | 40 | 11252008 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE | 2008-11-25 | 2008 | 11 |
| 3 | PHOENIX | AZ | GILBERT | AZ | HYDROCODONE | 20 | 6122009 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE | 2009-06-12 | 2009 | 6 |
| 4 | PHOENIX | AZ | GILBERT | AZ | HYDROCODONE | 10 | 10022009 | HYDROCODONE BITARTRATE HEMIPENTAHYDRATE | 2009-10-02 | 2009 | 10 |

</div></div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [19]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="n">drug</span> <span class="o">=</span> <span class="n">pills</span><span class="o">.</span><span class="n">groupby</span><span class="p">([</span><span class="s1">'year'</span><span class="p">,</span><span class="s1">'DRUG_NAME'</span><span class="p">])[</span><span class="s1">'QUANTITY'</span><span class="p">]</span><span class="o">.</span><span class="n">sum</span><span class="p">()</span>
<span class="n">drug</span> <span class="o">=</span> <span class="n">drug</span><span class="o">.</span><span class="n">compute</span><span class="p">()</span>
<span class="n">drug_df</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">DataFrame</span><span class="p">(</span><span class="n">data</span><span class="o">=</span><span class="n">drug</span><span class="p">)</span>
```

</div> </div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [20]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="n">sns</span><span class="o">.</span><span class="n">lineplot</span><span class="p">(</span><span class="n">data</span><span class="o">=</span><span class="n">drug_df</span><span class="p">,</span> <span class="n">x</span><span class="o">=</span><span class="s1">'year'</span><span class="p">,</span> <span class="n">y</span><span class="o">=</span><span class="s1">'QUANTITY'</span><span class="p">)</span>
```

</div> </div></div></div></div><div class="jp-Cell-outputWrapper"><div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser"></div><div class="jp-OutputArea jp-Cell-outputArea"><div class="jp-OutputArea-child"><div class="jp-OutputPrompt jp-OutputArea-prompt">Out[20]:</div><div class="jp-RenderedText jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/plain">```
<Axes: xlabel='year', ylabel='QUANTITY'>
```

</div></div><div class="jp-OutputArea-child"><div class="jp-OutputPrompt jp-OutputArea-prompt"></div><div class="jp-RenderedImage jp-OutputArea-output ">![](data:image/png;base64,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R0ZGBiwWC0pLS/HZZ5+12Gbu3LlQKBStfnw+XyxP54z4ghKqnT70TjVgYLaF4YeIiCiG4hqA1qxZgylTpuDrr7/GihUrEAqFMGHCBLjd7nbfs3btWowfPx6ffvopvv32W4wdOxZXX301Nm/e3GI7i8WCioqKFj86XWKuovIEQqh3+9Ev3YQBWWZoVCxwSEREFEtxvQW2bNmyFo/nzJmDzMxMfPvttxgzZkyb73n55ZdbPH7mmWewePFifPTRRxg2bFjz8wqFAjabrcOPuaM5vEF4ghKKbGb0TmWBQyIios6QUEMNdrsdAJCamhrxe2RZhtPpbPUel8uFPn36IDc3F1dddVWrEaJE0OAOwC9JGJhtRp80hh8iIqLOkjABSAiBqVOn4oILLsDgwYMjft8LL7wAt9uNm266qfm54uJizJ07F0uWLMH8+fOh0+kwevRo7Nmzp819+P1+OByOFj+xVuPyQygEBuVYWd2ZiIiokymEECLeBwEAU6ZMwSeffIJ169YhNzc3ovfMnz8fd955JxYvXoxx48a1u50syxg+fDjGjBmDWbNmtXr9iSeewPTp01s9b7fbYbFYIj+JCDS4A/jmYAMM2iQMzLYg1ajp0P0TERH1VA6HA1arNaLv74QYAbr//vuxZMkSrFq1KuLw895772Hy5Mn417/+ddLwAwBKpRLnnXdeuyNA06ZNg91ub/45cuRI1OcQqaQkBTIsGgzuZWX4ISIiipO4ToIWQuD+++/HokWLsHr1auTn50f0vvnz5+NnP/sZ5s+fjyuvvDKiz9myZQuGDBnS5utarRZabee0mrDo1BjaK5nzfYiIiOIorgFoypQp+Oc//4nFixfDbDajsrISAGC1WqHX6wGER2fKysowb948AOHwc+utt2LmzJkYOXJk83v0ej2sVisAYPr06Rg5ciQKCwvhcDgwa9YsbNmyBa+99loczrI1hh8iIqL4iustsNdffx12ux0XX3wxsrOzm3/ee++95m0qKipw+PDh5sezZ89GKBTClClTWrzngQceaN6msbERd999NwYOHIgJEyagrKwMa9euxfnnn9+p50dERESJKWEmQSeSaCZRERERUWLocpOgiYiIiDoTAxARERH1OAxARERE1OMwABEREVGPwwBEREREPQ4DEBEREfU4DEBERETU4zAAERERUY/DAEREREQ9DgMQERER9TgMQERERNTjxLUbfKJqao/mcDjifCREREQUqabv7UjanDIAtcHpdAIA8vLy4nwkREREFC2n0wmr1XrSbdgNvg2yLKO8vBxmsxkKhaJD9+1wOJCXl4cjR46w0/wp8FpFjtcqcrxWkeO1ig6vV+Rida2EEHA6ncjJyYFSefJZPhwBaoNSqURubm5MP8NisfAfSIR4rSLHaxU5XqvI8VpFh9crcrG4Vqca+WnCSdBERETU4zAAERERUY/DANTJtFotHn/8cWi12ngfSsLjtYocr1XkeK0ix2sVHV6vyCXCteIkaCIiIupxOAJEREREPQ4DEBEREfU4DEBERETU4zAARWnGjBk477zzYDabkZmZieuuuw67du1qsY0QAk888QRycnKg1+tx8cUX44cffmixjd/vx/3334/09HQYjUZcc801OHr0aKvP++STTzBixAjo9Xqkp6fj+uuvj+n5daTOularV6+GQqFo82fjxo2dcq5nqjN/r3bv3o1rr70W6enpsFgsGD16NFatWhXzc+xInXm9Nm3ahPHjxyM5ORlpaWm4++674XK5Yn6OHaWjrtUbb7yBiy++GBaLBQqFAo2Nja0+q6GhAZMmTYLVaoXVasWkSZPa3C5Rdea1evrppzFq1CgYDAYkJyfH8Kxio7Ou1cGDBzF58mTk5+dDr9ejf//+ePzxxxEIBM78JARFZeLEiWLOnDni+++/F1u2bBFXXnml6N27t3C5XM3bPPvss8JsNouFCxeKbdu2iZtvvllkZ2cLh8PRvM0999wjevXqJVasWCE2bdokxo4dK8466ywRCoWat/m///s/kZKSIl5//XWxa9cusXPnTvH+++936vmeic66Vn6/X1RUVLT4ufPOO0Xfvn2FLMudft6nozN/rwoKCsQVV1whtm7dKnbv3i3uvfdeYTAYREVFRaee85norOtVVlYmUlJSxD333CN27twp/vOf/4hRo0aJG264odPP+XR11LV66aWXxIwZM8SMGTMEANHQ0NDqsy677DIxePBg8dVXX4mvvvpKDB48WFx11VWdcZodojOv1R/+8Afx4osviqlTpwqr1doJZ9exOutaLV26VNx+++3is88+E/v27ROLFy8WmZmZ4le/+tUZnwMD0Bmqrq4WAMSaNWuEEELIsixsNpt49tlnm7fx+XzCarWKv/71r0IIIRobG4VarRYL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)</div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [21]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="c1">#Introducing the drug name as hue to show each drug performance</span>
<span class="n">plt</span><span class="o">.</span><span class="n">figure</span><span class="p">(</span><span class="n">figsize</span><span class="o">=</span><span class="p">(</span><span class="mi">10</span><span class="p">,</span><span class="mi">5</span><span class="p">))</span>
<span class="n">sns</span><span class="o">.</span><span class="n">lineplot</span><span class="p">(</span><span class="n">data</span><span class="o">=</span><span class="n">drug_df</span><span class="p">,</span> <span class="n">x</span><span class="o">=</span><span class="s1">'year'</span><span class="p">,</span> <span class="n">y</span><span class="o">=</span><span class="s1">'QUANTITY'</span><span class="p">,</span> <span class="n">hue</span><span class="o">=</span><span class="s1">'DRUG_NAME'</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s1">'Quantity of Pills Delivered by Drug Name'</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">legend</span><span class="p">(</span><span class="n">loc</span><span class="o">=</span><span class="mi">0</span><span class="p">)</span>
```

</div> </div></div></div></div><div class="jp-Cell-outputWrapper"><div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser"></div><div class="jp-OutputArea jp-Cell-outputArea"><div class="jp-OutputArea-child"><div class="jp-OutputPrompt jp-OutputArea-prompt">Out[21]:</div><div class="jp-RenderedText jp-OutputArea-output jp-OutputArea-executeResult" data-mime-type="text/plain">```
<matplotlib.legend.Legend at 0x7f5dd3aa9dd0>
```

</div></div><div class="jp-OutputArea-child"><div class="jp-OutputPrompt jp-OutputArea-prompt"></div><div class="jp-RenderedImage jp-OutputArea-output ">![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAA1cAAAHUCAYAAADWedKvAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjcuMiwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy8pXeV/AAAACXBIWXMAAA9hAAAPYQGoP6dpAACidUlEQVR4nOzdd3wU1frH8c+mNxIIIY0WegglQZAqSAlVil4U9KICYrmiXiwoYkERELDDDwEbTRTQixQLVboCopiA9JLQUkiAFBLSNvP7Y2EhJEACCRvC9/167UvPmTMzzySbsE/OzHNMhmEYiIiIiIiIyA2xs3UAIiIiIiIiZYGSKxERERERkWKg5EpERERERKQYKLkSEREREREpBkquREREREREioGSKxERERERkWKg5EpERERERKQYKLkSEREREREpBkquREREREREioGSKxGxqS1btvDAAw8QEBCAk5MTAQEB9OvXj23bttk6tDxiYmJ4++23iYiIyLft7bffxmQy5embOnUqs2bNujnBFeD06dM8+OCD+Pr6YjKZuPfee684tn379phMJuvL1dWV0NBQPvnkE3Jzc63jTCYTb7/9trW9bt06TCYT69ats/YV9LW4XpfGZWdnR7ly5ahduzYPPPAA//vf//LEVlSDBg0iKCgoT19QUBCDBg26saBtZNasWZhMJqKjo686btCgQXh4eJR4PCX5vStJF+Lu1q1bvm3R0dGYTCY++OADG0QmIrcKB1sHICK3r//7v//j+eefp3nz5rz33ntUr16do0eP8umnn9KyZUumTZvGk08+aeswAUtyNXr0aIKCgggLC8uz7fHHH8/3YWzq1Kn4+PjY7MP6mDFjWLRoETNmzKBWrVp4e3tfdXzNmjX55ptvADh58iTTp0/nhRdeIDY2lokTJwKwefNmqlSpUuKxXymutLQ0oqKiWLx4MQ888ABt27blxx9/xMvLq1jOtWjRIjw9PYvlWHJzv3fFbcWKFaxZs4aOHTvaOhQRucUouRIRm/jtt994/vnn6dGjB4sWLcLB4eKvowcffJD77ruPoUOH0qRJE+68804bRnptVapUuelJx7X8888/1KpViwEDBhRqvKurKy1btrS2u3fvTnBwMFOmTGHs2LE4Ojrm2X6zXB4XWJLZmTNn8thjj/Hkk0+yYMGCYjlXkyZNiuU4RWE2m8nJycHZ2fmmn7uk3ej3zjAMMjIycHV1LelQ86hbty45OTm88sorbNu2rdhmYkXk9qDbAkXEJsaPH4/JZGLatGl5EisABwcHpk6dah13QUG3ckHBt6J9+umntGvXDl9fX9zd3WnUqBHvvfce2dnZeca1b9+ehg0bsm3bNtq2bYubmxs1a9ZkwoQJ1luX1q1bZ03wBg8ebL3d6cItcpefPygoiF27drF+/Xrr2KCgIM6ePUv58uV56qmn8l1DdHQ09vb2vP/++1f9up0+fZqhQ4dSuXJlnJycqFmzJq+//jqZmZnW45hMJlavXs2ePXus57/01r3CcHR0pGnTpqSnp5OQkADkvy2wsNasWUP79u2pWLEirq6uVKtWjb59+5Kenl7kY10wePBgevTowffff8+RI0es/YZhMHXqVMLCwnB1daVChQrcf//9HD58+JrHvPS2wISEBJycnHjzzTfzjdu7dy8mk4nJkydb++Li4njqqaeoUqUKTk5O1KhRg9GjR5OTk2Mdc+F789577zF27Fhq1KiBs7Mza9euBeDPP/+kd+/eeHt74+LiQpMmTfjuu+/ynX/Lli20adMGFxcXAgMDGTlyZL739bXs2rWLTp064e7uTqVKlXj22WfzfD86depEcHAwhmHk2c8wDGrXrs0999xTpPNd6krfO5PJxLPPPsv06dOpX78+zs7OzJ49u8DbT+Hi1/Py22+/+OIL6tati7OzMyEhIXz77bdX/N1REEdHR8aNG8dff/11zcQ9ISGBoUOHEhISgoeHB76+vnTs2JGNGzcWGOv777/PxIkTCQoKwtXVlfbt27N//36ys7N59dVXCQwMxMvLi/vuu4+TJ0/mO9+CBQto1aoV7u7ueHh40LVrV/7+++9CXZeI3BxKrkTkpjObzaxdu5ZmzZpdccanatWqNG3alNWrV1/X8xmHDh3i3//+N19//TU//fQTQ4YM4f333y8wsYmLi2PAgAE8/PDDLF26lO7duzNy5Ejmzp0LwB133MHMmTMBeOONN9i8eTObN2/m8ccfL/DcixYtombNmjRp0sQ6dtGiRXh4ePDYY4/xzTffkJycnGefqVOn4uTkxGOPPXbFa8rIyKBDhw7MmTOHF198kZ9//pmHH36Y9957j3/9618ABAQEsHnzZpo0aULNmjWt57/jjjuu62vo4OBAhQoVirzvBdHR0dxzzz04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wGu59JoNo3DVAjdv3mzcd999RvXq1Q1nZ2ejYsWKxt13320sXbq0UHEB1pe7u7tRs2ZN4/777ze+//77PFXXLjVjxgyjRYsWhru7u+Hq6mrUqlXLePTRR40///zTOqYw1QIvSE5ONlxdXQ3A+OKLLwo8Z0JCgvHf//7XqFGjhuHo6Gh4e3sbTZs2NV5//XXj7NmzhmFcu9pcZGSk0a9fP8PX19dwdHQ0/P39jY4dOxrTp0/PM+63334zWrZsaTg7Oxv+/v7Gyy+/bHz++eeFrhbo7u5u7Nixw2jfvr3h6upqeHt7G08//bQ1zsvNmjXLAIz58+df9diXup7vHWA888wzBR4vNjbWuP/++w1vb2/Dy8vLePjhh40///yzwAqen3/+uVG7dm3DycnJqFu3rjFjxgyjT58+RpMmTQoVd0E/CwkJCYanp2e+719mZqYxfPhwo3LlyoaLi4txxx13GIsXL873/rrS9/7Cz9D333+fp3/mzJkGYGzbti1P/+LFi40OHToYnp6ehrOzs1G9enXj/vvvN1avXn3NaxORm8NkGFco2SP5mEwmFi1axL333gtYFvMbMGAAu3btynP/PVietbr8AeNOnTrh6enJokWLblbIIrecOXPmMHDgQF555RUmTpxo63BKTFZWFkFBQdx1110FLhQrUlr07duXLVu2EB0dnafK4a0iKSmJunXrcu+99/L555/bOhwRKeN0W+ANaNKkCWazmZMnT17zGaqoqCjWrl2b7x5xEcnr0UcfJTY2lldffRV3d3dGjRpl65CKVUJCAvv27WPmzJnEx8frNmEplTIzM9m+fTt//PEHixYt4qOPProlEqu4uDjGjRtHhw4dqFixIkeOHOHjjz8mNTU1z/ObIiIlRcnVNZw9e5aDBw9a21FRUURERODt7U3dunUZMGAAjz76KB9++CFNmjQhMTGRNWvW0KhRozyVhWbMmEFAQMBVKw+KiMWIESMYMWKErcMoET///DODBw8mICCAqVOnXrP8uogtxMbG0rp1azw9PXnqqad47rnnbB1SoTg7OxMdHc3QoUM5ffo0bm5utGzZkunTp1tL44uIlCTdFngN69ato0OHDvn6Bw4cyKxZs8jOzmbs2LHMmTOHEydOULFiRVq1asXo0aOtD6/n5uZSvXp1Hn300SuW6hURERERkVubkisREREREZFioHWuREREREREioGSKxERERERkWKgghYFyM3NJSYmhnLlymEymWwdjoiIiIiI2IhhGKSmphIYGHjNBcmVXBUgJiaGqlWr2joMEREREREpJY4dO0aVKlWuOkbJVQHKlSsHWL6Anp6eNo5GRERERERsJSUlhapVq1pzhKtRclWAC7cCenp6KrkSEREREZFCPS6kghYiIiIiIiLFQMmViIiIiIhIMVByJSIiIiIiUgz0zNV1MgyDnJwczGazrUOR24y9vT0ODg5aJkBERESklFFydR2ysrKIjY0lPT3d1qHIbcrNzY2AgACcnJxsHYqIiIiInKfkqohyc3OJiorC3t6ewMBAnJycNIMgN41hGGRlZZGQkEBUVBR16tS55mJ2IiIiInJzKLkqoqysLHJzc6latSpubm62DkduQ66urjg6OnLkyBGysrJwcXGxdUgiIiIiggpaXDfNFogt6f0nIiIiUvroE5qIiIiIiEgxUHIlIiIiIiJSDJRciYiIiIiIFAMlV7eRQYMGce+99+brX7duHSaTia+//hp3d3cOHjyYZ3tMTAwVKlRg0qRJAAQFBWEymTCZTLi6uhIUFES/fv1Ys2ZNnv2io6Ot40wmE15eXrRs2ZIff/wxXwznzp3jrbfeol69ejg7O+Pj48P999/Prl278o1NSUnh9ddfJzg4GBcXF/z9/QkPD+eHH37AMAzruF27dtGvXz8qVaqEs7MzderU4c0338xXQr+w13PB7Nmzad68Oe7u7pQrV4527drx008/Ffg1bdiwYb610MqXL8+sWbMKPP+lrwkTJhR4fhEREREpnZRciVWvXr3o2rUrAwcOJDc319r/5JNP0qRJE/773/9a+9555x1iY2PZt28fc+bMoXz58oSHhzNu3Lh8x129ejWxsbFs3bqV5s2b07dvX/755x/r9szMTMLDw5kxYwZjxoxh//79/PLLL5jNZlq0aMGWLVusY5OSkmjdujVz5sxh5MiRbN++nQ0bNtC/f39eeeUVkpOTAdiyZQstWrQgKyuLn3/+mf379/Puu+8ye/ZsOnfuTFZWVp4YC3s9w4cP56mnnqJfv35ERkbyxx9/0LZtW/r06cOUKVPyXfuhQ4eYM2fONb/2F85/6eu555675n4iIiIiUooYpcS7775rAMawYcOuOGbhwoVGeHi44ePjY5QrV85o2bKlsXz58jxjZs6c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)</div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell   "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [52]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span><span class="c1">#i dont like the scientific notation on the y-axis so lets try to change that to show millions instead (with the help of Perplexity)</span>

<span class="c1"># Format y-axis to show millions first we import matplotlibs ticker and use a function to set format the y-axis</span>

<span class="kn">import</span> <span class="nn">matplotlib.ticker</span> <span class="k">as</span> <span class="nn">ticker</span>


<span class="n">plt</span><span class="o">.</span><span class="n">figure</span><span class="p">(</span><span class="n">figsize</span><span class="o">=</span><span class="p">(</span><span class="mi">10</span><span class="p">,</span><span class="mi">5</span><span class="p">))</span>
<span class="n">ax</span> <span class="o">=</span> <span class="n">sns</span><span class="o">.</span><span class="n">lineplot</span><span class="p">(</span><span class="n">data</span><span class="o">=</span><span class="n">drug_df</span><span class="p">,</span> <span class="n">x</span><span class="o">=</span><span class="s1">'year'</span><span class="p">,</span> <span class="n">y</span><span class="o">=</span><span class="s1">'QUANTITY'</span><span class="p">,</span> <span class="n">hue</span><span class="o">=</span><span class="s1">'DRUG_NAME'</span><span class="p">)</span>

<span class="c1"># Function to format y-axis in millions</span>
<span class="k">def</span> <span class="nf">millions_formatter</span><span class="p">(</span><span class="n">x</span><span class="p">,</span> <span class="n">pos</span><span class="p">):</span>
    <span class="k">return</span> <span class="sa">f</span><span class="s1">'</span><span class="si">{</span><span class="n">x</span><span class="o">/</span><span class="mf">1e6</span><span class="si">:</span><span class="s1">.1f</span><span class="si">}</span><span class="s1">M'</span>

<span class="c1"># Apply the formatter to the y-axis</span>
<span class="n">ax</span><span class="o">.</span><span class="n">yaxis</span><span class="o">.</span><span class="n">set_major_formatter</span><span class="p">(</span><span class="n">ticker</span><span class="o">.</span><span class="n">FuncFormatter</span><span class="p">(</span><span class="n">millions_formatter</span><span class="p">))</span>

<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s1">'Quantity of Pills Delivered by Drug Name'</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">legend</span><span class="p">(</span><span class="n">loc</span><span class="o">=</span><span class="mi">0</span><span class="p">)</span>

<span class="c1"># Adjust y-label to reflect the new scale</span>
<span class="n">plt</span><span class="o">.</span><span class="n">ylabel</span><span class="p">(</span><span class="s1">'Quantity (Millions)'</span><span class="p">)</span>

<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>

<span class="c1">#Nice :)</span>
```

</div> </div></div></div></div><div class="jp-Cell-outputWrapper"><div class="jp-Collapser jp-OutputCollapser jp-Cell-outputCollapser"></div><div class="jp-OutputArea jp-Cell-outputArea"><div class="jp-OutputArea-child"><div class="jp-OutputPrompt jp-OutputArea-prompt"></div><div class="jp-RenderedImage jp-OutputArea-output ">![](data:image/png;base64,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KtbW1mRkZDzxuACdOnVCURS9D76DBg1i2LBhtGjRgnXr1vHdd99x6tQp6tevz/Xr1/M9dpkyZejQoQPLli3L9v5bsmQJZmZmuruOsbGx1KlThy1btjBmzBg2bdrEwIEDmTJlCu+88062sefMmcPOnTv53//+x6ZNm/D392fXrl00aNCA+Ph4FixYwPr16wkMDKRHjx56ycbp06dp3rw58fHxLF26lAULFnD8+HEmTpyY72vLzMykXbt2NG/enHXr1jFkyBC+//57evTooeszdOhQzp07x44dO/SO3bRpExcvXuSDDz7I9/lyktPPDmDdunXMnz+fMWPGsGXLFho2bFigcX/44QfeffddatSowZo1a/jyyy8ZP358gf8Y0aNHD1566SW+/PLLXBPd27dvAzB27Fg2btzIkiVL8PPzo0mTJjmec968eRw4cIB58+axaNEizp49S8eOHRk4cCBxcXEsXryY6dOns3379my/XyZPnswbb7xBlSpV+O233/jpp59ISkqiYcOGnD59ukDXJ4QoQooQQhRzsbGxCqD07Nkz1349evRQACUuLk5RFEXp27ev4u3tna3f2LFjldx+/anVaiUzM1NZvny5YmxsrNy+fVu3r3HjxgqgHDlyRO+YKlWqKK1bt9Zth4aGKoCyZMmSfJ2/atWqSuPGjbP1vXjxomJkZKTMnDlT15aamqqULl1a6d+//2OvQVEUZcGCBQqg/Pbbb3rt06ZNUwBl69atetdVtWrVXMd7tG9mZqaSmZmpREdHKyNHjlQApVu3brp+gDJ27Fjd9q5duxRA2bVrl67t0e/FH3/8oQBKWFhYvmLJKa7H2bRpkwIo06ZNUxRFUQ4dOqQAyjfffKPX78qVK4qlpaUyYsQIXVtO7yVvb2+lb9++uu2QkJBs39esrCzF3d1d6dq1q65t0KBBio2NjXL58mW98f73v/8pgHLq1ClFURQlIiJCAZRy5copGRkZen39/f2VmjVrKpmZmXrtHTp0UNzc3BS1Wq0oivbfhKWlpRIbG6sXk7+/vwIoERERj/1+3b9uQJk9e7Ze+6RJkxRA2b9/v6Io2n8zfn5+SufOnfX6tW3bVilXrpyi0WhyPU9Bf3aKon1/2dvb6/37VJSc32eK8uD7ef/fpFqtVlxdXZW6devq9bt8+bJiamqa4++O3OLevn27Aijffvut3vm+/vrrxx6flZWlZGZmKs2bN1deffXVbLEGBATofpaKoiizZs1SAKVTp0564wwbNkwBlISEBEVRFCUqKkoxMTFRPvzwQ71+SUlJiqurq9K9e/c8r00I8WzInTEhxHNDuTdF6kme2Th+/DidOnWidOnSGBsbY2pqSp8+fVCr1Zw/f16vr6urK3Xq1NFrq1Gjht4UqsLi5+dHhw4d+O6773TX9+uvv3Lr1i2GDBmS67E7d+7E2tqa119/Xa/9/tS6R+9iFMSpU6cwNTXF1NQUd3d3vvnmG3r16sXChQufeEyAwMBAzMzMePfdd1m2bFm+pgvml/LIFLoNGzagUql46623yMrK0r1cXV0JCAgo8N2Rtm3b4urqqrsrCtrCDtHR0Xp3MDds2EDTpk1xd3fXO2/btm0B2LNnj964nTp1wtTUVLf933//cfbsWd2dtofHaNeuHTExMZw7dw6AXbt20bx5c1xcXHTHGxsb693Vyo9HnyV88803deMDGBkZMWTIEDZs2EBUVBSgvVO6efNmBg8e/NTPUT36s7uvWbNmT3wn9ty5c8TGxtK9e3e9di8vLxo0aFDg8Zo3b06rVq346quvSEpKemy/BQsWUKtWLSwsLDAxMcHU1JQdO3Zw5syZbH3btWuHkdGDj2qVK1cGyDbt8377/e/9li1byMrKok+fPnrvDwsLCxo3blzg97YQouhIMiaEKPacnJywsrIiIiIi136RkZFYWlpSunTpAo0fFRVFw4YNuXbtGrNnz2bfvn2EhoYyb948AFJTU/X65zS+ubl5tn6FZejQoVy4cIFt27YB2qlLQUFBeU4lvHXrFq6urtk+CDs7O2NiYsKtW7eeOKZy5coRGhrK0aNH+ffff4mPj+fnn39+6mp35cqVY/v27Tg7O/PBBx9Qrlw5ypUrx+zZs59qXECXLLu7uwNw/fp1FEXBxcVFl1jefx0+fJibN28WaHwTExN69+7N2rVriY+PB7Ql5N3c3GjdurWu3/Xr1/nzzz+znbNq1aoA2c7r5uamt31/+uSnn36abYzBgwfrjXH/PfConNpyu65H3/P3j3/4PTRgwAAsLS1ZsGABoH2fWlpa5jqVNr8e/dnd9+j3piDux/5wonpfTm35MW3aNG7evPnYcvYzZszg/fffp27duqxevZrDhw8TGhpKmzZtcvz98WhVUzMzs1zb09LSgAfvkZdffjnbe2TVqlUFfm8LIYqOVFMUQhR7xsbGNGvWjE2bNnH16tUcnxu7evUq//zzj17JeAsLC12hioc9+kFk3bp1JCcns2bNGry9vXXtOa0RZgjNmjWjWrVqzJ07FxsbG44dO6Z7Pi03pUuX5siRIyiKopeQ3bhxg6ysLJycnJ44JgsLC2rXrv3Ex+emYcOGNGzYELVazdGjR3VLGri4uNCzZ88nHjckJASVSkWjRo0AbZKvUqnYt29fjtUJn6RiYf/+/fn6669ZuXIlPXr0ICQkhGHDhmFsbKzr4+TkRI0aNZg0aVKOYzyacDyaTN//uQUHB+ue/XtUpUqVAO17IDY2Ntv+nNoeJysri1u3buklZPePf7jN3t6evn37smjRIj799FOWLFnCm2++iYODQ77P9TiP/uzuy+mOm4WFBUC2f/uP/ru/H3tOzwYW5PvzsMDAQN544w1mzJhBu3btsu3/+eefadKkCfPnz9drz+1O2pO4/x75448/9H6nCSGKH7kzJoQoEUaOHImiKAwePBi1Wq23T61W8/7776NWqxk6dKiu3cfHhxs3buh92MrIyGDLli16x9//QPfwh29FUZ5qyt39sfJ7tyyvO2sfffQRGzduJDg4GBcXF7p165bnmM2bN+fu3busW7dOr3358uW6/cWZsbExdevW1d2hPHbs2BOPtWTJEjZt2sQbb7yBl5cXAB06dEBRFK5du0bt2rWzvapXr17g81SuXJm6deuyZMkSfv31V9LT0+nfv79enw4dOuiWEsjpvI8mY4+qVKkSFSpUIDw8PMfja9euja2tLQBNmzZlx44dev8G1Gp1gcv731//675ff/0V0FYXfdhHH33EzZs3ef3114mPj89zKm1+5PSzy839KognTpzQaw8JCdHbrlSpEq6urtkqUEZFRXHw4MEnjnfixIlkZGQwfvz4bPtUKlW2JP/EiRMcOnToic+Xk9atW2NiYsLFixcf+x4RQhQPcmdMCFEiNGjQgFmzZjF06FBeeeUVhgwZgpeXl27R50OHDjFu3DhatmypO6ZHjx6MGTOGnj178tlnn5GWlsacOXOyJXMtW7bEzMyMN954gxEjRpCWlsb8+fO5c+fOE8dbrlw5LC0t+eWXX6hcuTI2Nja4u7s/9oN29erVWblyJatWrcLPzw8LCwu9ZOCtt94iODiYvXv38uWXX+qmJeWmT58+zJs3j759+xIZGUn16tXZv38/kydPpl27drRo0eKJr6+oLFiwgJ07d9K+fXu8vLxIS0tj8eLFAPmKNzU1lcOHD+u+vnTpEuvWrWPDhg00btxYN4UOtO+pd999l/79+3P06FEaNWqEtbU1MTEx7N+/n+rVq/P+++8X+BoGDBjAoEGDiI6Opn79+rq7VPd99dVXbNu2jfr16/PRRx9RqVIl0tLSiIyM5K+//mLBggV5rkP3/fff07ZtW1q3bk2/fv3w8PDg9u3bnDlzhmPHjvH7778D2mqeISEhNGvWjDFjxmBlZcW8efNITk7O9/WYmZnxzTffcPfuXV5++WUOHjzIxIkTadu2La+88ope34oVK9KmTRs2bdrEK6+8QkBAQL7PU5CfXW5cXV1p0aIFU6ZMoVSpUnh7e7Njxw7WrFmj18/IyIjx48czaNAgXn/9dQYMGEB8fDzjx4/Hzc1N71mtgvD19eX999/PcWpthw4dmDBhAmPHjqVx48acO3eOr776Cl9fX71lDZ6Wj48PX331FaNGjeLSpUu0adOGUqVKcf36df7++2+sra1zTBaFEAZgsNIhQgjxBA4ePKh07dpVcXFxUYyMjBRAsbCwUDZu3Jhj/7/++ksJDAxULC0tFT8/P2Xu3Lk5VjP8888/lYCAAMXCwkLx8PBQPvvsM10Ft4ersj2u6ltO1fZWrFih+Pv7K6ampnqVBXM6f2RkpNKqVSvF1tZWAXKs5NavXz/FxMREuXr1at7fqHtu3bqlvPfee4qbm5tiYmKieHt7K8HBwUpaWppevyepppgXnqCa4qFDh5RXX31V8fb2VszNzZXSpUsrjRs3VkJCQvIVF6B7WVtbK35+fsrrr7+u/P7773pV6R62ePFipW7duoq1tbViaWmplCtXTunTp49y9OhRXZ/8VFO8LyEhQbG0tFQAZeHChTmeMy4uTvnoo48UX19fxdTUVHF0dFReeuklZdSoUcrdu3cVRcm7Gl94eLjSvXt3xdnZWTE1NVVcXV2VZs2aKQsWLNDrd+DAAaVevXqKubm54urqqnz22WfKDz/8kO9qitbW1sqJEyeUJk2aKJaWloqjo6Py/vvv6+J81NKlSxVAWblyZa5jP+xJfnaA8sEHH+Q4XkxMjPL6668rjo6Oir29vfLWW28pR48ezbHC6Q8//KCUL19eMTMzUypWrKgsXrxY6dy5s1KzZs18xZ3Tv4W4uDjFzs4u288vPT1d+fTTTxUPDw/FwsJCqVWrlrJu3bps76/H/ezv/xv6/fff9dqXLFmiAEpoaKhe+7p165SmTZsqdnZ2irm5ueLt7a28/vrryvbt2/O8NiHEs6FSlMeUKBJCiBJg+fLl9O3blxEjRjBt2jRDh1NkMjIy8PHx4ZVXXslxYV8hiouuXbty+PBhIiMj9apAlhTx8fFUrFiRLl268MMPPxg6HCHEc06mKQohSrQ+ffoQExPDyJEjsba2ZsyYMYYOqVDFxcVx7tw5lixZwvXr1xk5cqShQxIim/T0dI4dO8bff//N2rVrmTFjRolIxGJjY5k0aRJNmzaldOnSXL58mZkzZ5KUlKT3/KkQQhQVuTMmhBDF2NKlS+nfvz9ubm6MHTuWQYMGGTokIbKJjIzE19cXOzs73nzzTebOnatXQbK4unPnDn369CE0NJTbt29jZWVFvXr1GD9+PHXr1jV0eEKIF4AkY0IIIYQQQghhAFLaXgghhBBCCCEMQJIxIYQQQgghhDAAScaEEEIIIYQQwgCkmmIh0Gg0REdHY2tri0qlMnQ4QgghhBBCCANRFIWkpCTc3d3zXEBekrFCEB0djaenp6HDEEIIIYQQQhQTV65coWzZsrn2kWSsENja2gLab7idnZ2BoxFCCCGEEEIYSmJiIp6enrocITeSjBWC+1MT7ezsJBkTQgghhBBC5OvxJSngIYQQQgghhBAGIMmYEEIIIYQQQhiAJGNCCCGEEEIIYQDyzNgzoigKWVlZqNVqQ4ciXjDGxsaYmJjIsgtCCCGEEMWMJGPPQEZGBjExMaSkpBg6FPGCsrKyws3NDTMzM0OHIoQQQggh7pFkrIhpNBoiIiIwNjbG3d0dMzMzuUMhnhlFUcjIyCAuLo6IiAgqVKiQ5+KDQgghhBDi2ZBkrIhlZGSg0Wjw9PTEysrK0OGIF5ClpSWmpqZcvnyZjIwMLCwsDB2SEEIIIYRACng8M3I3QhiSvP+EEEIIIYof+YQmhBBCCCGEEAYgyZgQQgghhBBCGIAkY0IIIYQQQghhAJKMicfq168fXbp0yda+e/duVCoVP/30E9bW1vz33396+6OjoylVqhSzZ88GwMfHB5VKhUqlwtLSEh8fH7p3787OnTv1jouMjNT1U6lU2NvbU69ePf78889sMaSmpjJ27FgqVaqEubk5Tk5OvP7665w6dSpb38TEREaNGoW/vz8WFha4urrSokUL1qxZg6Ioun6nTp2ie/fulClTBnNzcypUqMDo0aOzLUmQ3+u5b9myZdSpUwdra2tsbW1p1KgRGzZsyPF7Wq1atWxr0Tk4OLB06dIcz//wa+rUqTmeXwghhBBCFE+SjIkn1rFjR1q3bk3fvn3RaDS69nfffZeaNWvy0Ucf6dq++uorYmJiOHfuHMuXL8fBwYEWLVowadKkbONu376dmJgYjhw5Qp06dejatSv//vuvbn96ejotWrRg8eLFTJgwgfPnz/PXX3+hVqupW7cuhw8f1vWNj4+nfv36LF++nODgYI4dO8bevXvp0aMHI0aMICEhAYDDhw9Tt25dMjIy2LhxI+fPn2fy5MksW7aMli1bkpGRoRdjfq/n008/ZdCgQXTv3p3w8HD+/vtvGjZsSOfOnZk7d262a7948SLLly/P83t///wPvz788MM8jxNCCCGEEMWIUkxMnjxZAZShQ4cqiqIoGRkZyogRI5Rq1aopVlZWipubm9K7d2/l2rVruY6zZMkSBcj2Sk1N1fXp27evAiiDBg3Kdvz777+vAErfvn3zHXtCQoICKAkJCdn2paamKqdPn9Y7v0ajUZLTMw3y0mg0+b6uvn37Kp07d87WvmvXLgVQ7ty5o9y4cUNxdnZWvv76a0VRtN9/Ozs7JTIyUtff29tbmTlzZrZxxowZoxgZGSlnz55VFEVRIiIiFEA5fvy4rk9iYqICKHPmzNG1TZ06VVGpVEpYWJjeeGq1Wqldu7ZSpUoV3XW+//77irW1dY7vm6SkJCUzU/s9qVKlilK7dm1FrVbr9QkLC1NUKpUyderUAl/PoUOHssV+3/DhwxVTU1MlKipK73v62WefKZ6ennrvF3t7e2XJkiV5nj83Ob0PhRBCiBeOWq0o4b8pyo9tFGVxO0VZ1VtRNgxXlF1TFOXvhYpyap2iROxXlBvnFCX5lra/EAWUW27wqGKxzlhoaCg//PADNWrU0LWlpKRw7NgxRo8eTUBAAHfu3GHYsGF06tSJo0eP5jqenZ0d586d02t7dG0lT09PVq5cycyZM7G0tAQgLS2NFStW4OXlVUhXlrPUTDVVxmwp0nM8zumvWmNlVng/9jJlyvD999/zxhtvEBAQwMcff8zs2bPx9vbO89ihQ4cyYcIE1q9fz4gRI7Ltz8zMZOHChQCYmprq2n/99VdatmxJQECAXn8jIyM+/vhjevXqRXh4ODVq1GDlypX06tULd3f3bOPb2NgAcPz4cU6fPs2vv/6arQR8QEAALVq0YMWKFXz++ecFup4VK1ZgY2PDoEGDsvX95JNPmDFjBqtXr2bYsGG69mHDhvHzzz8zd+5cPv3001zPJ4QQQogCiNgLW0dDTFj+jzEyASsnsC4D1qXv/bcMWN9r0+27918za1CpiuwSxPPH4MnY3bt36dWrFwsXLmTixIm6dnt7e7Zt26bX99tvv6VOnTpERUXlmjCpVCpcXV1zPW+tWrW4dOkSa9asoVevXgCsWbMGT09P/Pz8nuKKni8bNmzQJS33PfpMU5cuXejevTtt2rShQ4cO9OvXL19jOzo64uzsTGRkpF57/fr1MTIyIjU1FY1Go3sm677z58/TtGnTHMesXLmyro+7uzt37tzB398/1zjOnz+vd2xOY+7fv7/A13P+/HnKlSuHmZlZtr7u7u7Y29vrzn2flZUVY8eO5YsvvuCdd97B3t4+x3N9/vnnfPnll3ptGzZsoEmTJnnGKYQQQrxQbpyBbWPhwr0/hJvZQoOhUNoPkm/ee8Xde937OuUmpCWAJgvuxmpf+WFieS8xc8o7cbN2AhPzortuUSIYPBn74IMPaN++PS1atNBLxnKSkJCASqXCwcEh1353797F29sbtVpNYGAgEyZMoGbNmtn69e/fnyVLluiSscWLFzNgwAB2796d6/jp6emkp6frthMTE3Pt/yhLU2NOf9W6QMcUFktT4wL1b9q0KfPnz9drO3LkCG+99ZZe2+jRo1m+fDmjR48u0PiKoqB65C9Iq1atwt/fn/PnzzNs2DAWLFiAo6NjvscDbUL+8NdPI6cYi7LvwIEDmTFjBtOmTWPy5Mk5HvvZZ59lS3o9PDzydV4hhBDihZAUC7smwfGfQdFo73K91B8afw42ZfI+PisdUm49kqjd1E/aHv46K1X7SriifeWHuV32xC1b0nbvZeUIRgX7HCeKP4MmYytXruTYsWOEhobm2TctLY2RI0fy5ptvYmdn99h+/v7+LF26lOrVq5OYmMjs2bNp0KAB4eHhVKhQQa9v7969CQ4O1lXxO3DgACtXrswzGZsyZQrjx4/P1zXmRKVSFepUwaJkbW1N+fLl9dquXr2arZ+JiYnef/Pj1q1bxMXF4evrq9fu6elJhQoVqFChAjY2NnTt2pXTp0/j7OwMQMWKFTl9+nSOY549exaAChUqUKZMGUqVKsWZM2dyjaNixYoAnD59msDAwBzHfPS9k5/rqVixIvv37ycjIyPb3bHo6GgSExNzHNfExISJEyfSr18/hgwZkuO5nJycsv1chBBCCAGkJ8HBb7WvzHsVkSt3hObjwKkA/+80MQc7d+0rPzKSH0nUHpO4pdzb1mRBeqL2dftSPk6g0iZkeSZu9+7MWTjIlMkSwGAZwZUrVxg6dChbt27N9jzXozIzM+nZsycajYbvvvsu17716tWjXr16uu0GDRpQq1Ytvv32W+bMmaPX18nJifbt27Ns2TIURaF9+/Y4OTnlGXtwcDDDhw/XbScmJuLp6ZnncULf7NmzMTIyyrF8/n2NGzemWrVqTJo0SVcqv2fPnowaNYrw8HC958Y0Gg0zZ86kSpUqBAQEoFKp6NGjBz/99BNjx47N9txYcnIy5ubmBAYG4u/vz8yZM+nZs6fec2Ph4eFs376dKVOmFPh6evbsyZw5c/j++++zVTr83//+h6mpKV27ds1xrG7duvH1118/VdIvhBBCvFDUWXBsGeyeCsk3tG1l60CrCeBVL/djC4OZtfZVyifvvooCafE53GF7TOKWchtQtHfqUm5B3Nm8z2Fkqj9lMrfE7f7zbuKZM1gy9s8//3Djxg1eeuklXZtarWbv3r3MnTuX9PR0jI2NyczMpHv37kRERLBz585c74rlxMjIiJdffpkLFy7kuH/AgAG6uw/z5s3L15jm5uaYm8sc34JISkoiNjaWzMxMIiIi+Pnnn1m0aBFTpkzJ8w7PJ598Qrdu3RgxYgQeHh58/PHHrF+/no4dO/LNN99Qt25drl+/zuTJkzlz5gzbt2/XTf+bPHkyu3fvpm7dukyaNInatWtjamrKvn37mDJlCqGhoTg4OLBo0SJatWpF165dCQ4OxtXVlSNHjvDJJ58QFBSkV2Qjv9cTFBTE0KFD+eyzz8jIyKBLly5kZmby888/M3v2bGbNmpVrEj916lRat855Ouv98z/MysqqwP8+hBBCiBJPUeDcJtg+Fm7eexbb0Q9ajIPKnYrn3SGVCixLaV9Oec++QZ0Fqbfzl7gl39TebdNkQlKM9pUfplaP3G17NHF7qICJlROYZH8mXhScwZKx5s2bc/LkSb22/v374+/vz+eff66XiF24cIFdu3ZRunTpAp9HURTCwsKoXr16jvvbtGmjW0PqcR98xdMbM2YMY8aMwczMDFdXV+rVq8eOHTseW4jjYR06dMDHx4dJkybx3XffYWFhwc6dO5kyZQpffPEFly9fxtbWlqZNm3L48GGqVaumO7ZUqVIcPnyYqVOnMnHiRC5fvkypUqWoXr06X3/9ta5ARoMGDTh8+DDjx4+nXbt2JCYm4uXlRd++fQkODs6WfOf3embNmkWNGjWYP38+o0ePRqVSUatWLdatW0fHjh1zve5mzZrRrFkztm7d+tjv58MGDRrEggUL8vx+CiGEEM+Nq//AttFw+YB226q09pmwl/o/X8mCsQnYOGtf+ZGZ9lBy9vBzbzkkbndvgDpdO6UzPkr7yg8L+8c/36ZXebKMNumU591ypFLuVzkoBpo0aUJgYCCzZs0iKyuLrl27cuzYMTZs2ICLi4uun6Ojo+4ZnD59+uDh4aGbRjZ+/Hjq1atHhQoVSExMZM6cOfz0008cOHCAOnXqANCvXz/i4+NZt24d8KAAx/27Cl26dMHBwYGlS5fmK+7ExETs7e1JSEjIdmciLS2NiIgIfH1985yOKURRkfehEEKI58rtCNjxFZxao902sYB6g+GVYdokQeSfokDG3fwlbvf/q6jzHvdhKiOwdMx9mqTurltp7c+wON7RzKfccoNHFdsqElevXiUkJAQgW1GFXbt26Up4R0VF6T3jEx8fz7vvvktsbCz29vbUrFmTvXv36hKxnMjULiGEEEKIEiDlNuz9Gv5eqJ2GhwoC34Smo8Beqgo/EZUKzG21L8d8LO+k0eTwvNvjErc4SL2jrWaZcm9/XD5iMjLNNXFTrJy4hR2X06z4L9mSi/EaLsUlc/VOChs+fAUTY6O8z1FMFKs7YyWV3BkTxZ28D4UQQpRomWnw9/ew9xtIT9C2lWsGLb8C15wfRRHFhDpTm0Q/uhTA/fXcHn32LeNugU+RrJhzS7HjFvY4D1yFh7dhK04/F3fGhBBCCCHEC06jgZO/w84JD9bucqmmTcLKNzdsbCJ/jE3B1kX7ykF8SgYRN5OJvJVMRFwyV+PuEB8XTfKdWCwzb+OkSqQ0iTiqEnVfl1YlaL9WJWJGFtaqdKxVcXgRR4pDyZqmKsmYEEIIIYQofi7thq2jIfaEdtvOA5p9CTV6SDGIEuZuehaRN5O5dDOZyHuviFvJRNxMJj4lM4cjzAFvVCpvPOwt8XWyxtfJGtPS1jg6WWPjZI1TKUtMjVTadeUeuuNmZVvwgn+GJMmYEEIIIYQoPq6fhm1j4L9t2m1zO3jlY6j3PphaGjY28VipGWoibz1ItCJvapOtiJsp3LybnuuxrnYW+DhZ4etkg6+TFT6ltcmXp6MVFqZ5JN4WdtpX6XKFeDXPjiRjQgghhBDC8BKjYddkCPtFW/DByARqD4TGI7SFG4TBpWepuXI7hYibKQ/ubsVppxjGJKTleqyTjRm+Ttb4lLbG596dLl8na7xLW2Fl9uKmJC/ulQshhBBCCMNLT4IDs+HgXMhK1bZV6QzNx5bYux0lWZZaw9U7qY/c3dImXNfupKLJpfSfvaWpLsnSJl1W+DnZ4O1khZ2F6bO7iBJEkjEhhBBCCPHsqTPhn6Wwe6q2qh6AZ11oNRE8H78kkXh6Go1CdEIqkTdTiLh5V3un617yFXU7haxcMi4bcxN8HppK6Ot0705XaWtKWT9HC20/I5KMCSGEEEKIZ0dR4OxG2D4Wbv2nbXMsBy3Hg3+HEr3Yb3GiKAo3ktIf3Nl66A5X5K0UMrI0jz3WwtRIe2fr3pRCv3sJl4+TFWVszFHJz6jQSDImhBBCCCGejSuhsG00RB3Sbls5QZOR8FI/bQl0USCKonA7OUNvKmHkzRQu3Uzm8q1kUjLUjz3W1FiFl6OVbkqhbxnt3S0fJ2tc7SwwMpKE61mQZEzk6sqVK4wbN45NmzZx8+ZN3Nzc6NKlC2PGjMHBwYGGDRvi5ubG6tWrdcckJCRQrVo1+vbtS8uWLWnRogW7du3ilVde0fVJTk6mevXqdO7cmZkzZ5KYmMi0adNYvXo1kZGRODg4UK1aNQYPHsyrr76q+wvMqVOnGD9+PLt27SIxMREvLy969uxJcHAwVlZWuvF9fHy4fPkyABYWFri4uFCnTh3ee+89mjVrlu06ly1bxrx58zh16hRGRkbUrFmTESNG0KFDB12f3bt307RpU6pWrUp4eDjGxg+q+zg4ODBr1iz69euX7fwPmzJlCiNHjnzCn4YQQghRQt2+BNvHw+l12m0TSwj6ABoM1VbCE7lKSMnUe4Yr8taDZ7mS0rIee5yxkYqypSxznFLo7mCBibHRM7wKkRNJxsRjXbp0iaCgICpWrMiKFSvw9fXl1KlTfPbZZ2zatInDhw+zbNkyAgMD+eWXX+jVqxcAH374IY6OjowZMwYzMzM+/PBD+vXrR3h4ONbW1gCMGDECc3NzpkyZQnx8PK+88goJCQlMnDiRl19+GRMTE/bs2cOIESNo1qwZDg4OHD58mBYtWtCiRQs2btyIi4sLf//9N5988gk7d+5k165dmJk9mKv81Vdf8c4775CRkUFkZCQ///wzLVq0YMKECYwaNUrX79NPP2Xu3LlMnDiRLl26kJmZyc8//0znzp2ZPXs2Q4YM0fu+XLx4keXLl9O/f/9cv3/3z/8wW1vbp/qZCCGEECVK8i3YOx1CfwRNJqCCmr2gyRdg72Ho6IqV5PSsh+5uPbQm160UbidnPPY4lQrc7S3vlYa31ku8ypaywsxEEq7iTJIxQ1AUyEwxzLlNrfI9F/uDDz7AzMyMrVu3YmmpXdfDy8uLmjVrUq5cOUaNGsX8+fOZMmUKH374IU2bNiU0NJSVK1fy999/6xKjyZMns3nzZj7//HPmzp3Lrl27WLhwIQcPHsTCwoLhw4cTGRnJ+fPncXd3152/YsWKvPHGG1hYWKAoCgMHDqRy5cqsWbMGIyPtLxZvb28qVqxIzZo1mTlzJp9//rnueFtbW1xdXXVxN2rUCDc3N8aMGcPrr79OpUqVOHz4MN988w1z5szhww8/1B07adIk0tLSGD58OJ07d8bT01O378MPP2Ts2LG62B7n4fMLIYQQL5TMVDiyAPbNgPREbVv5FtDyK3CpatjYDCgtU83lWyn6z3HdS75uJOW+Fpezrbne3S2f0tb4lbHGKz9rcYliS5IxQ8hMgcnuefcrCl9Eg5l1nt1u377Nli1bmDRpki4Ru8/V1ZVevXqxatUqvvvuOz788EPWrl1Lnz59OHnyJGPGjCEwMFDX38LCguXLl1O/fn1atGjBxx9/zBdffEHt2rXRaDSsXLmSXr166SVi99nY2ABw/PhxTp8+za+//qpLxO4LCAigRYsWrFixQi8Zy8nQoUOZMGEC69evZ8SIEaxYsQIbGxsGDRqUre8nn3zCjBkzWL16NcOGDdO1Dxs2jJ9//pm5c+fy6aef5vWtFEIIIV4cGg2cWAU7J0LiVW2ba3VoOQHKNTVsbM9IRpaGK3dSspWFj7yZQnRCKkoupeEdrc0eurulXQT5fuVCa3P52P48kp+qyNGFCxdQFIXKlSvnuL9y5crcuXOHuLg4nJ2dmT9/PpUrV6Z69eo5PhNVu3ZtgoOD6dq1KzVr1uTLL78E4ObNm9y5cwd/f/9c4zl//rzuvI+LZ//+/Xlel6OjI87OzkRGRurGLVeunN70xvvc3d2xt7fXnfs+Kysrxo4dyxdffME777yDvb19juf6/PPPddd534YNG2jSpEmecQohhBAlzsVd2uIcsSe123ZlofloqN4djJ6vqXJqjcK1O6lcunlXN5XwfuJ1LT4VdS6l4W0tTB5UJ7x3d+t+1UJ7Syli8qKRZMwQTK20d6gMde5CoNz7s879whqLFy/GysqKiIgIrl69io+PT7ZjvvzyS7766itGjhyJiYlJjuM8TTz5HaMw+g4cOJAZM2Ywbdo0Jk+enOOxn332ma6gx30eHjI/XgghxHMm9l/YNgYu7tBum9tDw+FQdxCYWuZ+bDGm0SjEJqZlKw0fcSuZK7dTyFQ/PuGyMjPWPbvlc+8Ol++9O1yO1mZSGl7oSDJmCCpVvqYKGlL58uVRqVScPn2aLl26ZNt/9uxZSpUqhZOTE4cOHWLmzJls2rSJ6dOnM3DgQLZv357tF42pqfavPfcTMYAyZcpQqlQpzpw5k2s8FStWBOD06dN6UyAfjqdChQp5XtetW7eIi4vD19dXN+7+/fvJyMjIdncsOjqaxMTEHMc1MTFh4sSJ9OvXL1uBj/ucnJwoX758njEJIYQQJVJiNOycBGG/AAoYmcLLb0Ojz8C6tKGjyxdFUYi7txaXtkKhdhHkyHuLIKfnshaXmYkRPqWtspWF93WyxtlW1uIS+SPJmMhR6dKladmyJd999x0ff/yx3nNjsbGx/PLLL/Tp04e0tDT69u3LoEGDaNGiBRUrVqRatWp8//33vPfee3mex8jIiB49evDTTz8xduzYbM+NJScnY25uTmBgIP7+/sycOZOePXvqPTcWHh7O9u3bmTJlSp7nmz17NkZGRroEs2fPnsyZM4fvv/9er4AHwP/+9z9MTU3p2rVrjmN169aNr7/+mvHjx+d5XiGEEOK5kZYIB2bBoe8gK1XbVqULtBgLjn6GjCxfwq/Es/RgJOevJxF5M5nkXNbiMjHSrsXl80hZeB8nK9ztLWUtLvHUJBkTjzV37lzq169P69atmThxol5pew8PDyZNmsTIkSPRaDRMmzYN0FYt/Oabbxg+fDht2rTJcbrioyZPnszu3bupW7cukyZNonbt2piamrJv3z6mTJlCaGgoDg4OLFq0iFatWtG1a1eCg4NxdXXlyJEjfPLJJwQFBekV2QBISkoiNjaWzMxMIiIi+Pnnn1m0aBFTpkzR3bEKCgpi6NChfPbZZ2RkZOiVtp89ezazZs3Sq6T4qKlTp9K6desc990//8OsrKyws5P1VIQQQpRA6kz4ZynsngopN7VtXkHQaiKUrW3Q0PLjUtxd/rf1HH+d1P9/s5EKPO6txaV7lstJ+7WHg6WsxSWKliKeWkJCggIoCQkJ2falpqYqp0+fVlJTUw0Q2dOLjIxU+vXrp7i6uiqmpqaKp6en8uGHHyo3b95Udu/erRgbGyv79u3LdlyrVq2UZs2aKRqNRq8dUNauXZutf3x8vDJy5EilQoUKipmZmeLi4qK0aNFCWbt2rd4YJ06cULp27aqULl1aMTU1VcqVK6d8+eWXSnJyst543t7eCqAAipmZmeLl5aV0795d2blzZ47X+eOPPyq1a9dWLC0tFSsrK+WVV15RQkJC9Prs2rVLAZQ7d+5ku1ZAWbJkSY7nf/g1aNCgHM9f1Er6+1AIIYQBaTSKcmq9osyuqShj7bSvOS8pypkN2n3FXGxCqjJy9QnFL3ij4v35BsVn5Abl41XHla2nYpUL15OUtMwsQ4conjO55QaPUilKbgU2RX4kJiZib29PQkJCtrseaWlpRERE4Ovrm+uaVEIUJXkfCiGEeCJX/oatX8KVI9pt6zLQZCTU6gvGxbvyX0JqJgv2XGTJgQjSMrXPfrWo7Mxnrf2p5Gpr4OjE8yy33OBRMk1RCCGEEELou3URto+DMyHabRNLqD8EGgwF8+KdyKRlqll2MJLvdl8kITUTgNrepfi8rT8v+zgaODoh9EkyJoQQQgghtJJvwp7pcPRH0GSByggCe0HTUWDnZujocpWl1rD62FVmbrtAbGIaABVdbBjR2p/mlZ2luqEoliQZE0IIIYR40WWmwuHvYP8sSE/UtlVoBS3Gg0sVg4aWF0VR2HIqlq+3nONiXDIAHg6WfNyyIq/W9MBYKh6KYkySMSGEEEKIF5VGDeErYdckSLymbXOtoa2Q6NfYsLHlw6GLt5i2+SxhV+IBKGVlygdNy/NWPW8sTI0NG5wQ+SDJ2DMidVKEIcn7TwghRDb/7YBtY+H6Se22vSc0Gw3Vu4FR8S7nfio6gembz7HnfBwAlqbGvN3Ql3ca+WFnUbwLiwjxMEnGipipqfYXQkpKit7CyUI8SykpKcCD96MQQogXWOxJ2DYGLu7UbpvbQ6NPoM4gMC3eFXejbqXwzbZzrA+LBrSLMr9Z14shzcrjbFu8YxciJ5KMFTFjY2McHBy4ceMGoF30Vx4gFc+KoiikpKRw48YNHBwcMDaWKRtCCPHCSrgKOydB+ApAASNTqPMuNPoUrIp3lcG4pHTm7rzAr39HkanWzvboGODOJy0r4uNkbeDohHhykow9A66urgC6hEyIZ83BwUH3PhRCCPGCSUuA/TPh8HzI0lYZpOpr0HwMOPoaNrY8JKVlsnBfBIv2XSIlQw1Ao4plGNG6EtU87A0cnRBPT5KxZ0ClUuHm5oazszOZmZmGDke8YExNTeWOmBBCvIiyMuCfJbBnGqTc0rZ51dcW5yj7kmFjy0N6lppfDkcxd9d/3E7OACCgrD2ft/WnfjknA0cnROGRZOwZMjY2lg/FQgghhChaigKn18OO8XD7krbNqaK2TH2ltlCMH5dQaxTWh11jxrbzXL2TCoCfkzWfta5Em2qu8qiHeO5IMiaEEEII8byIOgxbR8PVv7Xb1s7QNBhq9gHj4vuxT1EUdp27wfTN5zgbmwSAi505w1pUpNtLZTExLt7VHYV4UsX3X6UQQgghhMifm//B9rFwdoN229QK6n+ofZnbGja2PPxz+TbTNp3j78jbANhZmPB+k/L0q++DpZnMKBLPN0nGhBBCCCFKqrtx2mfC/lkCmixQGUHN3tAkGOzcDB1drs5fT2L65nNsP3MdAHMTI/o18GFw4/LYW8lSLOLFUGzu+U6ZMgWVSsWwYcN0bYqiMG7cONzd3bG0tKRJkyacOnUqz7FWr15NlSpVMDc3p0qVKqxdu1Zvf79+/VCpVLz33nvZjh08eDAqlYp+/fo97SUJIYQQQhSNjBTY+zXMqQmhC7WJWMU28P5B6DSnWCdi1+JT+fT3cNrM2sv2M9cxUkHPlz3Z/VkTgttWlkRMvFCKRTIWGhrKDz/8QI0aNfTap0+fzowZM5g7dy6hoaG4urrSsmVLkpKSHjvWoUOH6NGjB7179yY8PJzevXvTvXt3jhw5otfP09OTlStXkpqaqmtLS0tjxYoVeHl5Fe4FCiGEEEIUBo0ajv0E39aCnRMhIwncAqHvn/DmKnCubOgIH+tOcgYTN5ym6f9288c/V9Eo0KaqK1s/bszUrjVws7c0dIhCPHMGT8bu3r1Lr169WLhwIaVKldK1K4rCrFmzGDVqFK+99hrVqlVj2bJlpKSk8Ouvvz52vFmzZtGyZUuCg4Px9/cnODiY5s2bM2vWLL1+tWrVwsvLizVr1uja1qxZg6enJzVr1iz06xRCCCGEeGKKAhe2w4KGEDIEkmLA3gteWwTv7ALfRoaO8LFSMrKYu/MCjabvYtH+CDKyNNTzc2Tt4Pos6P0S5Z1tDB2iEAZj8GTsgw8+oH379rRo0UKvPSIigtjYWFq1aqVrMzc3p3Hjxhw8ePCx4x06dEjvGIDWrVvneEz//v1ZsmSJbnvx4sUMGDAgz5jT09NJTEzUewkhhBBCFImYE/BTF/ilK9w4BRb22rXChoRCjW5gZPCPcznKVGv46fBlGn+9m/9tPU9SehZV3OxY2v9lVrxTj5pepfIeRIjnnEELeKxcuZJjx44RGhqabV9sbCwALi4ueu0uLi5cvnz5sWPGxsbmeMz98R7Wu3dvgoODiYyMRKVSceDAAVauXMnu3btzjXvKlCmMHz8+1z5CCCGEEE8l/op2KuKJVYACxmZQ511o+AlYORo6usfSaBQ2nozhm63niLyVAoCXoxWftKpIxxruGBnJWmFC3GewZOzKlSsMHTqUrVu3YmFh8dh+jy7upyhKngv+5fcYJycn2rdvz7Jly1AUhfbt2+PklPeq7sHBwQwfPly3nZiYiKenZ57HCSGEEELkKTUe9s+AwwtAna5tq/Y6NB8NpXwMGVme9l2IY9rms/x7TTtryMnGjI+aV6Dny16YmRTPO3hCGJLBkrF//vmHGzdu8NJLL+na1Go1e/fuZe7cuZw7dw7Q3ulyc3tQEejGjRvZ7nw9zNXVNdtdsNyOGTBgAEOGDAFg3rx5+Yrd3Nwcc3PzfPUVQgghhMiXrAw4+iPsmQ6p2jW38H4FWn0FHi/lfqyBnbgaz7TNZznw3y0AbMxNeLeRHwNf8cXaXFZSEuJxDPavo3nz5pw8eVKvrX///vj7+/P555/j5+eHq6sr27Zt0xXUyMjIYM+ePUybNu2x4wYFBbFt2zY+/vhjXdvWrVupX79+jv3btGlDRkYGoH22TAghhBDimVIUOL0Oto+HOxHaNqdK0PIrqNga8pgRZEiX4u7yzdbzbDwZA4CZsRFv1fPmg6blKG0jf7gWIi8GS8ZsbW2pVq2aXpu1tTWlS5fWtQ8bNozJkydToUIFKlSowOTJk7GysuLNN9/UHdOnTx88PDyYMmUKAEOHDqVRo0ZMmzaNzp07s379erZv387+/ftzjMPY2JgzZ87ovhZCCCGEeGYuH4KtX8K1o9ptGxdo+gUEvgXGxfeO0vXENGZtv8BvR6+g1iioVPBqTQ8+blERT0crQ4cnRIlRfP+VAyNGjCA1NZXBgwdz584d6taty9atW7G1tdX1iYqKwuihKkL169dn5cqVfPnll4wePZpy5cqxatUq6tat+9jz2NnZFel1CCGEEELouXkBto2Fcxu126bW0OAjCBoC5sW31HtCaiYL9lxkyYEI0jI1ADT3d+azNpXwd5XPU0IUlEpRFMXQQZR0iYmJ2Nvbk5CQIImdEEIIIR7v7g3YPRX+WQqKGlRGUKsPNAkGW1dDR/dYaZlqlh2M5LvdF0lIzQTgJe9SjGzrz8s+xbeyoxCGUJDcoFjfGRNCCCGEeC5kJMOh7+DALMi4q22r2BZajocylQwaWm6y1BpWH7vKrO0XiElIA6Ciiw2ftfanRWXnPCtcCyFyJ8mYEEIIIURR0agh7BfYNRmStEUucK8FrSaAzyuGjS0XiqKw5dR1/rf1HP/d0CaP7vYWfNyyIq/VKouxrBUmRKGQZEwIIYQQorApClzYBtvGQJy2UBgOXtB8LFR9DYyK75pbhy/dYtrmsxyPigfAwcqUIU3L81Y9byxMpdiZEIVJkjEhhBBCiMIUHQbbRkPEXu22hQM0+gzqvAMmxbfc++noRKZvOcvuc3EAWJoa83ZDX95p5IedhamBoxPi+STJmBBCCCFEYYiPgp0T4cQq7baxGdQdBA0/ActSho0tF1dup/DN1nOsD49GUcDESMUbdbz4sHl5nG0tDB2eEM81ScaEEEIIIZ5G6h3YNwOOfA/qdG1b9e7Q7Eso5W3Y2HJx8246c3f+xy9HLpOp1hbX7hjgzictK+LjZG3g6IR4MUgyJoQQQgjxJLLSIXQR7P1am5AB+DTUFudwr2nY2HJxNz2LhXsvsWjfJZIz1AA0rODE5238qeZhb+DohHixSDImhBBCCFEQigKn1sD28RB/WdtWxh9afgUVWkExLfeenqXm1yNRzN35H7eSMwAIKGvP5238qV/eycDRCfFikmRMCCGEECK/Ig/A1i8h+ph228YVmn4Bgb3AuHh+rFJrFNaHXWPGtvNcvZMKgJ+TNZ+2rkTbaq6yVpgQBlQ8f2sIIYQQQhQncedg+zg495d229QaXhkGQR+AWfF8vkpRFHadu8H0zec4G5sEgLOtOcNaVKRb7bKYGhff8vpCvCgkGRNCCCGEeJy7cbBrEhxbDooaVMbwUl9oEgw2zoaO7rH+uXyHaZvO8nfkbQBsLUx4v0k5+tf3xdJM1goToriQZEwIIYQQIieR++H3/pB8Q7tdqT20GAdlKho0rNxcuJ7E9C3n2Hb6OgDmJkb0q+/D+03K4WBlZuDohBCPkmRMCCGEEOJhigKH5sK2sdq7Yc5Vod3X4NPA0JE9VnR8KjO3nWf1satoFDBSQffangxtUQE3e0tDhyeEeAxJxoQQQggh7ktPgvUfwOn12u0aPaHDTDCzMmxcj3EnOYPvdv/HskOXycjSANCmqiuftq5EeWcbA0cnhMiLJGNCCCGEEKAt0rHqLbh5HoxMoc0UePntYlmqPiUjiyUHIlmw+yJJ6VkA1PNz5PM2/tT0KmXg6IQQ+SXJmBBCCCHEqXXaO2IZd8HWHbovB8+XDR1VNplqDatCrzB7xwXiktIBqOxmx+dtKtG4YhkpUy9ECSPJmBBCCCFeXOos2DEODn6r3fZpCK8vAZsyBg3rURqNwl//xvDN1vNE3EwGwNPRkk9bVaJjDXeMjCQJE6IkkmRMCCGEEC+muzfgjwEQuU+73WAoNBtT7BZv3n/hJtM2n+XktQQAnGzM+LBZBd6o44WZiawVJkRJVrx+2wghhBBCPAtX/obf+kBSDJjZQJfvoEpnQ0el58TVeKZvPsf+/24CYG1mzLuNyjGwoS825vIRTojngfxLFkIIIcSLQ1EgdBFsDgZNJjhVgh4/F6u1wyJuJvO/LefYeDIGADNjI3rV82JI0/KUtjE3cHRCiMIkyZgQQgghXgwZKbBhGJxYpd2u+ip0mgvmxaME/I3ENGbtuMCq0CuoNQoqFbwa6MHHLSvi6Vg8S+sLIZ6OJGNCCCGEeP7duqidlnj9X1AZQ6sJUG9wsShbn5Cayfd7LrL4QARpmdq1wpr7O/Np60pUdrMzcHRCiKIkyZgQQgghnm/nNsGaQZCeANbO0G0p+DQwdFSkZapZfiiS73ZfJD4lE4BaXg6MbFuZOr6OBo5OCPEsSDImhBBCiOeTRg27p8Der7XbnnWh2zKwczNoWFlqDWuOXWPm9vPEJKQBUMHZhhFt/GlR2VnWChPiBSLJmBBCCCGePym3YfVAuLhTu133PWg5AUzMDBaSoihsPX2dr7ec478bdwFwt7fg45YVea1WWYxlrTAhXjiSjAkhhBDi+XLtGPzWFxKiwNQKOs6BGt0MGtKRS7eYtvksx6LiAXCwMmVI0/K8Vc8bC1Njg8YmhDAcScaEEEII8fz4Zxn89SmoM8DRT1u23qWqwcI5E5PI9M1n2XUuDgBLU2MGvuLLu439sLMwNVhcQojiQZIxIYQQQpR8mWnaJOz4T9rtSu3h1flgYW+QcK7cTmHGtvOsC7uGooCJkYqedTz5qFkFnO0sDBKTEKL4kWRMCCGEECXbncvasvUxYaAygmZfQoOPwcjomYdy8246c3f+xy9HLpOpVgDoUMONT1tVwsfJ+pnHI4Qo3iQZE0IIIUTJ9d92WP02pN4Bq9LQ9Uco1/SZh3E3PYuFey+xaN8lkjPUADSs4MSI1v5UL2uYu3NCiOJPkjEhhBBClDwaDez7BnZNAhRwrwXdl4OD5zMNIz1Lza9Hopi78z9uJWcAUKOsPZ+38adBeadnGosQouSRZEwIIYQQJUtqPKwdBOc3a7df6gdtpoHps3sWS6NRWB9+jW+2nufqnVQAfJ2s+bRVJdpVd5W1woQQ+SLJmBBCCCFKjth/YdVbcCcCjM2hwwyo+dYzO72iKOw+F8e0zWc5G5sEgLOtOUNbVKB7bU9MjZ/9c2pCiJLLoL8x5s+fT40aNbCzs8POzo6goCA2bdqk269SqXJ8ff31148dc+nSpTkek5aWpuvTr18/VCoV7733XrbjBw8ejEqlol+/foV6rUIIIYR4SuGrYFELbSLm4AUDtz7TROxY1B16/HCY/ktDORubhK2FCSPaVGLPZ03pVddbEjEhRIEZ9M5Y2bJlmTp1KuXLlwdg2bJldO7cmePHj1O1alViYmL0+m/atImBAwfStWvXXMe1s7Pj3Llzem0WFvpTFzw9PVm5ciUzZ87E0tISgLS0NFasWIGXl9fTXpoQQgghCktWBmz5AkIXarfLt4DXFoKV4zM5/X83kpi++RxbT18HwMzEiP71fXi/STkcrMyeSQxCiOeTQZOxjh076m1PmjSJ+fPnc/jwYapWrYqrq6ve/vXr19O0aVP8/PxyHVelUmU79lG1atXi0qVLrFmzhl69egGwZs0aPD098xxfCCGEEM9IwjX4vS9cDdVuNx4JjUeAkXGRn/p0dCIL911ifdg1NAoYqaDbS54Ma1kBN3vLIj+/EOL5V2yeGVOr1fz+++8kJycTFBSUbf/169fZuHEjy5Yty3Osu3fv4u3tjVqtJjAwkAkTJlCzZs1s/fr378+SJUt0ydjixYsZMGAAu3fvznX89PR00tPTdduJiYl5xiSEEEKIAorYC38MgOQ47eLNry2Eiq2L9JSKorD3wk0W7bvEvgs3de2tq7rwWetKlHe2LdLzCyFeLAZPxk6ePElQUBBpaWnY2Niwdu1aqlSpkq3fsmXLsLW15bXXXst1PH9/f5YuXUr16tVJTExk9uzZNGjQgPDwcCpUqKDXt3fv3gQHBxMZGYlKpeLAgQOsXLkyz2RsypQpjB8/vsDXKoQQQoh8UBQ4OAe2jwNFAy7VocdP4OhbZKdMz1ITEhbNj/sjdIU5jI1UtKvuxjsNfalR1qHIzi2EeHGpFEVRDBlARkYGUVFRxMfHs3r1ahYtWsSePXuyJWT+/v60bNmSb7/9tkDjazQaatWqRaNGjZgzZw6gLeARHx/PunXr6Nq1KzVq1EBRFP7991/++OMPunTpgoODA0uXLs1xzJzujHl6epKQkICdnV3BvgFCCCGEeCA9CdYNhjMh2u2AN6D9DDCzKpLTJaRk8svfl1l6IJIbSdr/t1ubGdPjZS/6N/DB07FoziuEeH4lJiZib2+fr9zA4HfGzMzMdAU8ateuTWhoKLNnz+b777/X9dm3bx/nzp1j1apVBR7fyMiIl19+mQsXLuS4f8CAAQwZMgSAefPm5WtMc3NzzM3NCxyLEEIIIXIRd05btv7meTAyhbbToPYAKII1u67cTuHH/RH8dvQKKRlqAFzszOnfwJc36nhhb2la6OcUQohHGTwZe5SiKHp3nQB+/PFHXnrpJQICAp5ovLCwMKpXr57j/jZt2pCRkQFA69ZFOw9dCCGEEI/x7xpYPwQyk8HOA7ovh7K1C/00YVfiWbjvEptOxqC5NzfI39WWdxv50aGGO2YmUp5eCPHsGDQZ++KLL2jbti2enp4kJSXpntfavHmzrk9iYiK///4733zzTY5j9OnTBw8PD6ZMmQLA+PHjqVevHhUqVCAxMZE5c+YQFhb22LtexsbGnDlzRve1EEIIIZ4hdRZsHwuH5mq3fRtB18VgU6bQTqHRKOw4e4OFey/xd+RtXXujimV4p6Evr5R3QlUEd9+EECIvBk3Grl+/Tu/evYmJicHe3p4aNWqwefNmWrZsqeuzcuVKFEXhjTfeyHGMqKgojIwe/BUrPj6ed999l9jYWOzt7alZsyZ79+6lTp06j41DnvMSQgghDCDpurZa4uX92u0Gw6DZaDAunI8naZlqVh+7yo/7Irh0MxkAU2MVnQI8eLuhL5Xd5P//QgjDMngBj+dBQR7SE0IIIQQQdRh+6wt3Y8HMFrp8B1U6FcrQt+6ms/zQZX46fJnbydpHEewsTOhVz5t+9X1wsbMolPMIIUROSlQBDyGEEEK8QBQF/v4BtnwBmiwo4w89fganCnkfm4eLcXf5cX8Eq/+5SnqWBoCypSwZ+Iov3Wt7Ym0uH3uEEMWL/FYSQgghxLORkQx/DoWTv2u3q74Gnb4Fc5snHlJRFEIj7/DD3ktsP3Nd1x5Q1p53GvnRpqorJsZSlEMIUTxJMiaEEEKIonfrorZs/Y3ToDKGVhOh3vtPXLY+S61h86lYFu69RPjVBEA7VHN/F95t5MfLPqWkKIcQotiTZEwIIYQQRevsRlj7HqQngo0LdFsK3vWfaKjk9CxWhV5h8YEIrt5JBcDcxIiuL5Vl4Cu+lCvz5HfZhBDiWZNkTAghhBBFQ6OGXZNg373labyCtImYrWuBh7qemMbSg5H8cvgyiWlZADham9EnyJve9bwpbWNeiIELIcSzIcmYEEIIIQpf8i1YPQAu7dZu1xsMLb8CY9MCDXM2NpGFeyMICb9GplpbANrXyZq3G/rStVZZLExljVAhRMklyZgQQgghCte1f2BVH0i8CqZW2iId1V/P9+GKorD/v5ss3BfB3vNxuvY6Po6808iP5v7OGBnJ82BCiJJPkjEhhBBCFA5FgWPL4K/PQJ0BjuWg5y/gXDlfh2dkadhwIpof9l7ibGwSAEYqaFvdjXca+hHo6VCEwQshxLMnyZgQQgghnl5mKvz1KRz/Wbvt30G7kLOFfZ6HJqRmsuLvKJYciOB6YjoAVmbGdK/tycBXfPF0tCrKyIUQwmAkGRNCCCHE07kTCat6Q+wJUBlB8zHQYFieZeuv3klh8f5IVoVGkZyhBsDZ1px+DXzoVccbe6uCPV8mhBAljSRjQgghhHhyF7bB6rchLR6sSsPri8GvSa6HnLgaz8J9Efx1Mga1RluUo5KLLe808qNjgBvmJlKUQwjxYpBkTAghhBAFp9HA3q9h9xRAAY+XoPtysC/7mO4KO8/eYOG+SxyJuK1rb1jBibcb+tGogpMs0iyEeOFIMiaEEEKIgkm9A2sGwYUt2u3aA6DNVDDJvtZXWqaatcevsXDfJS7FJQNgYqSiU4A7bzf0o4q73bOMXAghihVJxoQQQgiRfzEn4Lfe2ufETCyg/Qyo2Stbt9vJGfx06DI/HY7k5t0MAGzNTXiznhf96vvgZm/5jAMXQojiR5IxIYQQQuRP2ArYMAyy0sDBG3r8DG419LpE3Ezmx/2X+OOfq6RlagDwcLCkfwMferzsia2FFOUQQoj7JBkTQgghRO6y0mFzMBz9UbtdoRW89gNYlgK0izT/c/kOP+y9xLYz11G0NTmo7mHPO438aFfNFRNjIwMFL4QQxddTJWPp6emYm2efHy6EEEKI50TCNfitD1w7CqigyUhoNAKMjFBrFLaciuWHvZcIuxKvO6S5vzPvNPKjrq+jFOUQQohcFCgZ27JlCytWrGDfvn1ERUWh0WiwsrKiVq1atGrViv79++Pu7l5UsQohhBDiWbq0B/4YACk3wcIBui6CCi1JTs/i96OXWXwgkqjbKQCYmRjRtZYHA1/xpbyzrWHjFkKIEkKlKPcnEzzeunXr+Pzzz0lISKBdu3bUqVMHDw8PLC0tuX37Nv/++y/79u3j0KFD9OvXjwkTJlCmTJlnEX+xkJiYiL29PQkJCdjZSVUoIYQQJZyiwIHZsGM8KBpwrQE9fuKGsSvLDkXy8+EoElIzAShlZUrvet70DvKhjK3MlhFCiILkBvlKxurUqcPo0aNp3749RkaPn/N97do1Zs+ejYuLC5988knBIy+hJBkTQgjx3EhLhHXvw9kN2u3AXlx4eRw/HIxhfVg0GWptUQ6f0lYMbOjH67XKYmkmizQLIcR9hZ6MidxJMiaEEOK5cOMMrHoLbv2HYmzGxZdGMzG2LrvP39R1qe1dircb+tGyigvGRvI8mBBCPKogucFTV1NUq9WcPHkSb29vSpUq9bTDCSGEEMIQ/l0N6z+EzGRSLF35wuQz1u11A25ipII21Vx5u6Eftbzk//VCCFFYCpyMDRs2jOrVqzNw4EDUajWNGzfm4MGDWFlZsWHDBpo0aVIEYQohhBCiSKgzYdsYOPwdAKGqGgy6M5jb2GFpakz32mUZ8Iov3qWtDRyoEEI8fwqcjP3xxx+89dZbAPz5559ERERw9uxZli9fzqhRozhw4EChBymEEEKIIpAUS/qKPphHHwFgXlYnvsnqjqONJZ818KFXXS8crMwMHKQQQjy/CpyM3bx5E1dXVwD++usvunXrRsWKFRk4cCBz5swp9ACFEEIIUfgu/bON0n+9i736NomKJZ9mvkeEU1OmNvSjc013zE2kKIcQQhS1AidjLi4unD59Gjc3NzZv3sx332mnNaSkpGBsLL+4hRBCiOJKo1HYc+4GUZtm8GbCQkxVas5pyvK963jeaN6IJhXLyCLNQgjxDBU4Gevfvz/du3fHzc0NlUpFy5YtAThy5Aj+/v6FHqAQQgghnk5appr1Ydf4ae9p3o2fRV/jQ6CCf+yaY/HaPGb4uBk6RCGEeCEVOBkbN24c1apV48qVK3Tr1g1zc+0Cj8bGxowcObLQAxRCCCHEk7mTnMHPhy+z7NBlbJMjWWA6k0rGV1GrjLnbaDwvNRkCcidMCCEMRtYZKwSyzpgQQoji5PKtZH7cH8FvR6+QlqmhlVEoM80WYE0qGmtnjLovB+8gQ4cphBDPpSJfZ2zHjh3s2LGDGzduoNFo9PYtXrz4SYYUQgghxFP65/IdFu69xJbTsSgKGKNmusN6uqf9oe3gVR+jbkvA1tWwgQohhACeIBkbP348X331FbVr19Y9NyaEEEIIw1BrFLadjuWHvZc4FhWva+9U3pSvsubiEHtQ21DvA2g5HoxNDROoEEKIbAqcjC1YsIClS5fSu3fvoohHCCGEEPmQmqHmj3+usGh/BJdvpQBgZmxEl5ruDKmYgNf29yDxGphaQ+e5UO01A0cshBDiUUYFPSAjI4P69esXysnnz59PjRo1sLOzw87OjqCgIDZt2qTb369fP1Qqld6rXr16eY67evVqqlSpgrm5OVWqVGHt2rV6+++P+95772U7dvDgwahUKvr16/fU1yeEEEIUtrikdL7Zeo6gqTsYvf4Ul2+lYG9pypCm5dn/eROmex/Fa31XbSJWujy8s1MSMSGEKKYKnIy9/fbb/Prrr4Vy8rJlyzJ16lSOHj3K0aNHadasGZ07d+bUqVO6Pm3atCEmJkb3+uuvv3Id89ChQ/To0YPevXsTHh5O79696d69O0eOHNHr5+npycqVK0lNTdW1paWlsWLFCry8vArl+oQQQojCcuF6Ep//cYIGU3fy7c7/iE/JxMvRiq86V+VQcDM+beaF847hsHE4qDPAvwO8swucZdkZIYQorgo8TTEtLY0ffviB7du3U6NGDUxN9eeez5gxI99jdezYUW970qRJzJ8/n8OHD1O1alUAzM3NcXXN/4PGs2bNomXLlgQHBwMQHBzMnj17mDVrFitWrND1q1WrFpcuXWLNmjX06tULgDVr1uDp6Ymfn1++zyeEEEIUFUVROHTpFov2RbDz7A1de00vB95t6Eerqq4YG6ngdgT81htiT4LKCFqMg/ofSdl6IYQo5gqcjJ04cYLAwEAA/v33X719T1PMQ61W8/vvv5OcnExQ0INyu7t378bZ2RkHBwcaN27MpEmTcHZ2fuw4hw4d4uOPP9Zra926NbNmzcrWt3///ixZskSXjC1evJgBAwawe/fuXGNNT08nPT1dt52YmJiPKxRCCCHyJ1Ot4a+TMSzcd4l/r2n/H6NSQasqLrzbyI+XvB0fdD6/Fda8A2nxYOUEry8Gv8aGCVwIIUSBFDgZ27VrV6EGcPLkSYKCgkhLS8PGxoa1a9dSpUoVANq2bUu3bt3w9vYmIiKC0aNH06xZM/755x/dYtOPio2NxcXFRa/NxcWF2NjYbH179+5NcHAwkZGRqFQqDhw4wMqVK/NMxqZMmcL48eOf7IKFEEKIx0hKy2RV6BUW748gOiENAAtTI7q95MmAV3zxdbJ+0FmjgT3TtC8U8KgN3ZeDvYdhghdCCFFgT7TO2H1Xr15FpVLh4fHkv/grVapEWFgY8fHxrF69mr59+7Jnzx6qVKlCjx49dP2qVatG7dq18fb2ZuPGjbz22uMfRn70Dp2iKDnetXNycqJ9+/YsW7YMRVFo3749Tk5OecYcHBzM8OHDdduJiYl4enrm53KFEEKIbKLjU1l6MJIVR6JISs8CwMnGjL5BPvSq542jtZn+ASm3Yc278N827fbLb0PryWCS8x8qhRBCFE8FTsY0Gg0TJ07km2++4e7duwDY2tryySefMGrUKIyMClYTxMzMjPLlywNQu3ZtQkNDmT17Nt9//322vm5ubnh7e3PhwoXHjufq6prtLtiNGzey3S27b8CAAQwZMgSAefPm5Stmc3Pzx96ZE0IIIfLrVHQCi/ZF8Gd4NFkaBYByZax5p6EfXWp6YGFqnP2gmHBY1RviL4OJBXSYBYFvPNvAhRBCFIoCJ2OjRo3ixx9/ZOrUqTRo0ABFUThw4ADjxo0jLS2NSZMmPVVAiqLoPY/1sFu3bnHlyhXc3Nwee3xQUBDbtm3Te25s69atjy3H36ZNGzIyMgDts2VCCCFEUVIUhT3n41i47xIH/rula6/n58i7jfxoUtEZI6PHPIN9/BdttcSsNCjlA91/ArcazyZwIYQQha7AydiyZctYtGgRnTp10rUFBATg4eHB4MGDC5SMffHFF7Rt2xZPT0+SkpJ0z2tt3ryZu3fvMm7cOLp27YqbmxuRkZF88cUXODk58eqrr+rG6NOnDx4eHkyZMgWAoUOH0qhRI6ZNm0bnzp1Zv34927dvZ//+/TnGYGxszJkzZ3RfCyGEEEUhPUvN+rBoFu27xPnr2pklxkYq2ld3452GflQva//4g7PSYdPn8M8S7XaF1vDa92BZ6hlELoQQoqgUOBm7ffs2/v7Z1yzx9/fn9u3bBRrr+vXr9O7dm5iYGOzt7alRowabN2+mZcuWpKamcvLkSZYvX058fDxubm40bdqUVatWYWtrqxsjKipKb2pk/fr1WblyJV9++SWjR4+mXLlyrFq1irp16z42Djs7uwLFLYQQQuRXfEoGvxyJYunBSOKStDM/rM2M6VnHi/4NfChbyiqPAa7Ab30g+higgqZfQMNPoYCPBQghhCh+VIqiKAU5oG7dutStW5c5c+botX/44YeEhoZy+PDhQg2wJEhMTMTe3p6EhARJ7IQQQgAQdSuFxQciWBV6hdRMNQCudhb0b+BDzzpe2Fua5jECcGk3/DEAUm6BhQN0/REqtCjSuIUQQjydguQGBb4zNn36dNq3b8/27dsJCgpCpVJx8OBBrly5wl9//fXEQQshhBDPg+NRd1i47xKb/43lXk0OKrvZ8W4jX9pXd8fMJB93tBQF9s+EnRNA0YBbgLZsfSmfIo1dCCHEs1XgZKxx48acP3+eefPmcfbsWRRF4bXXXmPw4MG4u7sXRYxCCCFEsXYnOYO//o1hzbFr/HP5jq69ccUyvNPQjwblS+e4xEqO0hJg3WA4u0G7HfgWtP8fmFoWQeRCCCEMqcDTFEV2Mk1RCCFePCkZWWw7fZ2QsGj2nI/TlaY3NVbROdCDtxv64u9awP8nXD8Nq96C2xfB2AzafQ21+kJ+EzkhhBAGV+jTFE+cOEG1atUwMjLixIkTufatUUNK7AohhHg+ZWRp2HchjvVh0Ww7fV33LBhAFTc7Oge606WmBy52FgUf/OQfEPIhZKaAXVnosRw8XirE6IUQQhQ3+UrGAgMDiY2NxdnZmcDAQFQqFTndUFOpVKjV6hxGEEIIIUomjUbh78jbhIRH89fJGOJTMnX7vEtb0TnAnU6B7pR3ts1llFyoM2HraDgyX7vt11RbqMO6dCFEL4QQojjLVzIWERFBmTJldF8LIYQQzzNFUTgVnUhIeDR/hkcTk5Cm2+dkY07HADc6B3oQUNY+/8+C5SQpFn7rC1fuVSJu+Km2dL2RrHsphBAvgnwlY97e3jl+LYQQQjxPIm4mExIWzfrwa1yKS9a121qY0LaaK50DPajnVxpjo0J4hivyAPzeD5JvgLkdvPo9+Ld7+nGFEEKUGPlKxkJCQvI9YKdOnZ44GCGEEOJZu5GYxp8nYggJu0b41QRdu7mJES0qu9AxwJ0mlcpgYVpId6sUBQ5/p52aqKjBuQr0+BlKlyuc8YUQQpQY+UrGunTpkq/B5JkxIYQQJUFCSiabT8WwPiyaQ5ducf8xaGMjFQ3KO9E5wJ1WVV2wtcjHwswFkX5XW6Tj1BrtdvVu0HE2mFkX7nmEEEKUCPlKxjQaTVHHIYQQQhSptEw1O87cYH3YNXafiyND/eD/bS95l6JTgDvtqrtRxta8aAK4eUFbtj7uLBiZQOspUOcdKVsvhBAvsAIv+iyEEEKUFJlqDQf+u0lIWDRbTsWSnPFg9kYlF1s6BbrTKcAdT0erog3kdIh2IeeMJLBxhe7Lwatu0Z5TCCFEsZevZGzOnDn5HvCjjz564mCEEEKIp6XRKByLusP6MG0p+lvJGbp9Hg6WdA7UlqIv8ILMT0KdBTu/ggOztdveDeD1JWDrUvTnFkIIUeyplJwWDHuEr69v/gZTqbh06dJTB1XSFGSVbSGEEEXjbGwi68OiCQmL5lp8qq69tLUZ7Wu40TnQnVpepZ6uFH1B3I2DP/pD5D7tdtAQaDEOjAv5OTQhhBDFSkFyg3yvMyaEEEIUN1dupxASHs36sGucv35X125tZkzre6XoG5QrjYmx0TMOLBR+6wNJ0WBqDV3mQdVXn20MQgghij15ZkwIIUSJEpeUzsYT0YSER3MsKl7XbmZsRFP/MnQO9KCZv3PhlaIvCEWBoz/CppGgyYTSFaDnL1Cm0rOPRQghRLGXr2Rs+PDhTJgwAWtra4YPH55r3xkzZhRKYEIIIcR9SWmZbDl1nfVh1zjw30009ybYG6kgqFxpOgd40LqaK/aWBpwCmJYAf42AEyu125U7Qed5YCHT14UQQuQsX8nY8ePHyczM1H39OM9sHr4QQojnXlqmmt3nbrA+LJodZ2+QkfWgFH2ApwOdA9zpUMMNZzsLA0aJ9m7Yv6thyxdw9zqojKDFeKj/oZStF0IIkat8FfAQuZMCHkIIUTjUGoVDF2+xPuwam/+NJSk9S7evXBlrugR60DHAHR+nYrJI8s3/4K9P4NJu7Xbp8tBxDvg0MGhYQgghDKfQC3gIIYQQRUVRFMKuxLM+LJoNJ2K4eTddt8/N3oJOAdpS9FXc7IrPDIzMNNg/A/bPBHUGGJtDo0+hwVAwKaJFo4UQQjx38p2MDRgwIF/9Fi9e/MTBCCGEeHFcuJ50rxJiNFG3U3TtDlamtK/uRqcAd172ccTIqJgkYPf9twP++hRu31vKpVxzaPc1lC5n2LiEEEKUOPlOxpYuXYq3tzc1a9ZEZjYKIYR4EtfiU/nzXgJ2JiZR125pakyrqi50DnTnlfJlMDN5xqXo8yMxRvtc2Kk12m1bN2gzBap0kWfDhBBCPJF8J2PvvfceK1eu5NKlSwwYMIC33noLR0fHooxNCCHEc+B2cgYbT8YQEnaN0Mg7unYTIxVNKpWhU6AHLSo7Y2VWTGfOa9QQugh2TICMJG2BjjqDoOkXUilRCCHEUylQAY/09HTWrFnD4sWLOXjwIO3bt2fgwIG0atWq+MzjNwAp4CGEEPqS07PYdlpbin7fhZtk3atFr1JBXV9HOgV40LaaK6WszQwcaR6u/QMbPoaYcO22x0vQYSa4BRg2LiGEEMVWQXKDJ66mePnyZZYuXcry5cvJzMzk9OnT2NjYPFHAJZ0kY0IIARlZGvacj2N92DW2n7lOWuaDUvTVPOzoHOBBhwA33OwtDRhlPqXGw84JEPojoIC5PbQYCy/1AyMDLCYthBCixHgm1RRVKhUqlQpFUdBoNHkfIIQQ4rmj1ij8HXGbkPBr/HUyloTUTN0+XydrXSXEcmVKyB/rFAVO/qF9Niz5hratRg9oNRFsnA0bmxBCiOdOgZKxh6cp7t+/nw4dOjB37lzatGmDkVExfNhaCCFEoVMUhX+vJbI+7Bp/nojmeuKDUvTOtuZ0DHCnc6A71T3sS9YU9pv/wcbhELFHu126ArT/BvwaGzYuIYQQz618J2ODBw9m5cqVeHl50b9/f1auXEnp0qWLMjYhhBDFyKW4u6wPi+bP8Ggu3UzWtdtZmNCuuhudAt2p61sa4+JWij4vj64ZZmKhXTOs/keyZpgQQogile9nxoyMjPDy8qJmzZq5/qVzzZo1hRZcSSHPjAkhnlexCWlsOKEtRX/yWoKu3cLUiBaVXegU4E7jSmUwNymhz1H9tx02fgp3IrTb5Vto1wxz9DNsXEIIIUqsInlmrE+fPiVruokQQognEp+SwaZ/Y1kfdo0jEbe5/yc7YyMVDSs40TnQnZZVXLExL6al6PMjMQa2BMOptdptWzdoMxWqdJY1w4QQQjwzBVr0WQghxPMpJSOL7WduEBIWzZ7zN8hUP5g08bJPKToFuNOuuhulbUr4tD11lnbNsJ0TH6wZVvc9aBIsa4YJIYR45krwnzWFEEI8jUy1hv0XbrI+7BpbT18nJUOt2+fvakvnQA86BrhRtpSVAaMsRFf/gQ3DIPaEdtujNnSYIWuGCSGEMJh8JWPvvfceo0aNwtPTM8++q1atIisri169ej11cEIIIQqXRqNw9PIdQsKvsfFEDHdSHpSi93S0pHOAB50C3anoYmvAKAtZajzs+AqOLgYUsLCHFuOgVj+QSsBCCCEMKF//FypTpgzVqlWjbdu2zJ8/n9DQUK5du8atW7f477//CAkJYcSIEXh5eTFr1ixq1KiRr5PPnz+fGjVqYGdnh52dHUFBQWzatAmAzMxMPv/8c6pXr461tTXu7u706dOH6OjoXMdcunSpbg20h19paWm6Pv369UOlUvHee+9lO37w4MGoVCr69euXr2sQQojiTlEUTkcnMmXTGV6ZtpPu3x/i58NR3EnJxMnGjH71fVgzuD57P2vKp60rPT+JmKLAid9gbm04em/x5ho9YchRqD1AEjEhhBAGl687YxMmTODDDz/kxx9/ZMGCBfz77796+21tbWnRogWLFi2iVatW+T552bJlmTp1KuXLlwdg2bJldO7cmePHj1O2bFmOHTvG6NGjCQgI4M6dOwwbNoxOnTpx9OjRXMe1s7Pj3Llzem0WFhZ6256enqxcuZKZM2diaWkJQFpaGitWrMDLyyvf1yCEEMXV5VvJhIRFExIezYUbd3XttuYmtK7mSudAd4L8SmNi/BwmJTcv3FszbK9226mids0w30aGjUsIIYR4SL6fGXN2diY4OJjg4GDi4+O5fPkyqampODk5Ua5cuSeqtNixY0e97UmTJjF//nwOHz7MwIED2bZtm97+b7/9ljp16hAVFZVrwqRSqXB1dc313LVq1eLSpUusWbNGN6VyzZo1eHp64ucnJY2FECXTjaQ0Np6IYX1YNGFX4nXtZiZGNPd3pnOgO00qOWNhWkJL0eclMxX2fQMHZj+0Zthn99YMMzN0dEIIIYSeJyrg4eDggIODQ6EGolar+f3330lOTiYoKCjHPgkJCahUqjzPfffuXby9vVGr1QQGBjJhwgRq1qyZrV///v1ZsmSJLhlbvHgxAwYMYPfu3bmOn56eTnp6um47MTEx94sTQogilJiWyeZ/YwkJi+bgxZto7hVCNFJBg/JOdApwp3U1V+wsTA0baFG7sB3++gTuRGq3y7e8t2aYr0HDEkIIIR7H4NUUT548SVBQEGlpadjY2LB27VqqVKmSrV9aWhojR47kzTffzHXxNH9/f5YuXUr16tVJTExk9uzZNGjQgPDwcCpUqKDXt3fv3gQHBxMZGYlKpeLAgQOsXLkyz2RsypQpjB8//omuVwghCkNappqdZ7Wl6Heeu0FGlka3r6aXA50D3Glfw50ytiW8FH1+JEbD5mA4vU67besObadC5U6yZpgQQohiTaUoipJ3t6KTkZFBVFQU8fHxrF69mkWLFrFnzx69hCwzM5Nu3boRFRXF7t2781zJ+mEajYZatWrRqFEj5syZA2gLeMTHx7Nu3Tq6du1KjRo1UBSFf//9lz/++IMuXbrg4ODw2LXVcroz5unpma9VtoUQ4kllqTUcvHiL9WHRbDkVy930LN2+Cs42dA50p1OAB16ln5NS9HlRZ0Howntrht0FlbF2zbCmwWD+nBQhEUIIUeIkJiZib2+fr9zA4HfGzMzMdAU8ateuTWhoKLNnz+b7778HtIlY9+7diYiIYOfOnQVOdoyMjHj55Ze5cOFCjvsHDBjAkCFDAJg3b16+xjQ3N8fc/AX4a7MQwuAUReFYVDwhYdfYeDKGm3czdPs8HCzpGOBO50B3/F1tn+jZ3RLr6tF7a4ad1G6XfRnazwC3/FXzFUIIIYoDgydjj1IURXfX6X4iduHCBXbt2kXp0qWfaLywsDCqV6+e4/42bdqQkaH9cNO6desnD1wIIQrR+etJrA+7Rkh4NFdup+raHa3NaF/djU6B7rzkVQojoxcoAQNIvXNvzbAlaNcMc7i3ZlhfKVUvhBCixClwMjZu3Dj69++Pt7f3U5/8iy++oG3btnh6epKUlKR7Xmvz5s1kZWXx+uuvc+zYMTZs2IBarSY2NhYAR0dHzMy0VbH69OmDh4cHU6ZMAWD8+PHUq1ePChUqkJiYyJw5cwgLC3vsXS9jY2POnDmj+1oIIQzlyu0U/jwRTUhYNGdjk3Tt1mbGtKrqSqdAd14p74Tp81iKPi/31wzbOgqS47RtAW9AywlgU8awsQkhhBBPqMDJ2J9//snEiRNp3LgxAwcO5LXXXsu2hld+Xb9+nd69exMTE4O9vT01atRg8+bNtGzZksjISEJCQgAIDAzUO27Xrl00adIEgKioKIwe+mtofHw87777LrGxsdjb21OzZk327t1LnTp1HhuHPOclhDCUm3fT+etkDCFh0Ry9fEfXbmqsokklbSn65v4uWJq9wH8sijuvXTMscp9226midkqib0PDxiWEEEI8pScq4HHixAmWLFnCr7/+SkZGBj179mTAgAG8/PLLRRFjsVeQh/SEECIpLZOtp64TEh7N/v9uor5Xi16lgiC/0nQKcKdtNTfsrZ7zUvR5yUyFvf/TrhmmydSuGdZ4BAR9KGuGCSGEKLYKkhs8VTXFrKws/vzzT5YsWcLmzZupVKkSb7/9Nv369cPe3v5Jhy1xJBkTQuQlLVPN7nM3CAmPZseZG6Q/VIq+Rll7OgW40zHAHRe7J5tp8Ny5sA3++vTBmmEVWmnXDCvlY8iohBBCiDw9s2qKGo2GjIwM0tPTURQFR0dH5s+fz+jRo1m4cCE9evR4muGFEKJEu1+KPiQ8mi3/xpL0UCl6vzLWdApwp3OgB75O1gaMsphJjIbNI+H0eu22nQe0mQqVO8qaYUIIIZ47T5SM/fPPPyxZsoQVK1Zgbm5Onz59mDdvnq5E/TfffMNHH30kyZgQ4oWjLUV/h/Vh0fz1SCl6N3sL3R2wqu52L1Yp+ryos+DvH2DXpAdrhtV7H5qMlDXDhBBCPLcKPE2xRo0anDlzhlatWvHOO+/QsWPHbFUI4+LicHFxQaPRPGaU54tMUxTixaYoCmdikggJj+bP8GiuxeuXom9X3ZVOAR7U9n4BS9Hnx5VQ2Pix/pphHWaCa85LkgghhBDFWZFOU+zWrRsDBgzAw8PjsX3KlCnzwiRiQogX1+VbyYSERRMSHs2FG3d17dZmxrSu6krHF7kUfX6k3oHt4+GfpejWDGs5Hmr2kTXDhBBCvBAKnIwpikKpUqWytaempvL1118zZsyYQglMCCGKoxuJafx5IoaQ8GjCr8Tr2s1MjGhaqQydAz1o5u+MhekLXIo+L4oCJ1bBllGQclPbFvAmtJoA1k6GjU0IIYR4hgo8TdHY2JiYmBicnZ312m/duoWzszNqtbpQAywJZJqiEM+3+JQMNv0bS0hYNIcjbnH/t6aRChqUd6JTgDutq7liZ/GCl6LPj2xrhlWCDjPA5xXDxiWEEEIUkiKdpqgoSo4PnYeHh+Po6FjQ4YQQolhKychi2+nr/BkezZ7zcWSqH/zd6iXvUnQKcKdddTfK2JobMMoSJNuaYZb31gwbImuGCSGEeGHlOxkrVaoUKpUKlUpFxYoV9RIytVrN3bt3ee+994okSCGEeBYysjTsPR9HSHg0205fJzXzwZ1+f1dbOgW607GGO56OVgaMsgQ6v1W7Zlj8Ze12xTbQdpqsGSaEEOKFl+9kbNasWSiKwoABAxg/frzeos5mZmb4+PgQFBRUJEEKIURRUWsUjkTc4s/waP46GUtCaqZun5ejFZ0D3ekU4E4FFymvXmAJ17Rrhp0J0W7beUDb6eDfXtYME0IIIShAMta3b18AfH19qV+/Pqam8myEEKJkUhSFE1cTdKXobySl6/Y525rToYY7nQLdCShrL2uBPQl1Fvz9Peya/GDNsKDB0HgkmNsYOjohhBCi2MhXMpaYmKh7+KxmzZqkpqaSmpqaY18pYCGEKK4uXH+wFljkrRRdu52FCe2qu9EpwJ26fqUxlrXAntyVUNjwMVy/v2ZYnXtrhlUzbFxCCCFEMZSvZKxUqVK6CooODg45/qX4fmGPF7GaohCi+Lp6J4U/w7Wl6M/EJOraLU2NaVHFhU4B7jSq6IS5iZSifyopt2HHePhnGaCAZSloMR5q9pY1w4QQQojHyFcytnPnTl2lxF27dhVpQEII8bRu3k3nr5MxrA+L5p/Ld3TtpsYqGlUoQ6dAd1pUdsHavMAFZcWjFAXCV8LWLx+sGRbYC1p+JWuGCSGEEHnI1yeRxo0b67729fXF09Mz290xRVG4cuVK4UYnhBD5lJSWyZZT11kfdo2DF2+h1mhL0atUUM+3NJ0C3WlbzRUHKymjXmjizsGG4XB5v3a7jD+0nwE+DQwblxBCCFFCFPjPwr6+vjku+nz79m18fX1lmqIQ4plJy1Sz8+wNQsKi2XnuBhlZGt2+gLL2dAxwp0MNd1ztLQwY5XMoIwX2fg0Hv32wZliTz6HeB7JmmBBCCFEAhbbo8927d7GwkA88QoiilanWcOC/m4SER7P11HXupmfp9pV3tqFTgLYUvY+TtQGjfI6d33JvzbAo7XbFNtpy9aW8DRuXEEIIUQLlOxkbPnw4ACqVitGjR2Nl9WDRU7VazZEjRwgMDCz0AIUQQqNR+CfqDiFh0Ww8GcPt5AzdPg8HSzoEuNE5wIPKbrZSir6oJFy9t2bYn9ptu7LahZtlzTAhhBDiieU7GTt+/DigvTN28uRJzMweTEUxMzMjICCATz/9tPAjFEK8kBRF4XRMIiFh2lL00Qlpun2lrc1oX0Nbir6WVymMpBR90VFnwpEFsGsKZCbfWzPsA2j8uawZJoQQQjylfCdj96so9u/fn9mzZ8t6YkKIIhFxM5mQsGhCwq9xMS5Z125jbkLrqq50CnSnQbnSmBhLufQid+Xve2uG/avd9qwHHWaAS1XDxiWEEEI8Jwr8zNiSJUuKIg4hxAssNiGNDSeiCQmP5sTVBF27mYkRzf2d6RTgTlN/ZyxMZS2wZyLlNmwfB8eWabctS2lL1Qe+JWuGCSGEEIWowMlYcnIyU6dOZceOHdy4cQONRqO3/9KlS4UWnBDi+XUnOYNN/8YSEn6NIxG3UbSV6DE2UtGgvBOdA9xpVdUFWwtTwwb6IlEUCF9xb82wW9q2wLfurRlW2rCxCSGEEM+hAidjb7/9Nnv27KF37964ubnJw/JCiHxLTs9i+5nrrA+LZu/5OLLurQUGUNu7FJ0D3Wlb3Q0nG3MDRvmCunEWNg6Hywe022UqQ4eZ4B1k2LiEEEKI51iBk7FNmzaxceNGGjSQRT2FEHlLz1Kz51wcIeHRbD9znbTMB3fTq7jZ0SnQnQ413ChbyiqXUUSRyUiBvdPvrRmWBaZW2uIcQR+AsdyVFEIIIYpSgZOxUqVK4ejoWBSxCCGeE2qNwuFLtwgJi2bTvzEkpj1YC8yntJV2LbBAd8o72xowSsG5zbDpswdrhlVqpy1X7+Bl2LiEEEKIF0SBk7EJEyYwZswYli1bprfWmBDixaYoCmFX4gkJj2bDiRjiktJ1+1zszOlYQ5uAVfewl+nNhpZwFTZ9Dmc3aLftykK76do1w4QQQgjxzBQ4Gfvmm2+4ePEiLi4u+Pj4YGqqP43l2LFjhRacEKL4O3896V4p+miibqfo2u0tTWlXXbsWWB1fR4xlLTDDe3TNMCOTB2uGmVkbOjohhBDihVPgZKxLly5FEIYQoiS5cjuFkHDtYsxnY5N07VZmxrSs4kKnAHcaViiDmYmUQS82oo5o1wy7cUq77RUE7WeASxXDxiWEEEK8wFSKoih5dxO5SUxMxN7enoSEBFkMWzy34pLS2XhvLbBjUfG6dlNjFY0rOtMp0J0WlZ2xMivw33hEUUq5DdvHwrHl2m1Lx3trhvWSNcOEEEKIIlCQ3EA+NQkhHishNZMtp2L5MzyaA//d5H4lepUKgvxK0znQnTZV3bC3kqp7xY6iQNivsG30gzXDar4FLWTNMCGEEKK4KHAyplarmTlzJr/99htRUVFkZGTo7b99+3ahBSeEePbSMtXsOHOD9WHX2H0ujgz1g1L0gZ4OdArQlqJ3trMwYJQiVzfOwIbhEHVQu+1cRTslUdYME0IIIYqVAidj48ePZ9GiRQwfPpzRo0czatQoIiMjWbduHWPGjCmKGIUQRSxTrWH/fzcJCYtm66lYkjPUun0VnG3oHOhOxwB3vEtLkYdiLac1w5qMhHqDZc0wIYQQohgq8AMDv/zyCwsXLuTTTz/FxMSEN954g0WLFjFmzBgOHz5coLHmz59PjRo1sLOzw87OjqCgIDZt2qTbrygK48aNw93dHUtLS5o0acKpU6fyHHf16tVUqVIFc3NzqlSpwtq1a/X29+vXD5VKxXvvvZft2MGDB6NSqejXr1+BrkWIkkajUThy6Raj1p6kzqTt9F8Sytrj10jOUOPhYMn7TcqxaWhDtn7ciCHNKkgiVtyd2wTz6sL+mdpErFJ7+OBvaDBUEjEhhBCimCrwnbHY2FiqV68OgI2NDQkJCQB06NCB0aNHF2issmXLMnXqVMqXLw/AsmXL6Ny5M8ePH6dq1apMnz6dGTNmsHTpUipWrMjEiRNp2bIl586dw9Y258ViDx06RI8ePZgwYQKvvvoqa9eupXv37uzfv5+6devq+nl6erJy5UpmzpyJpaUlAGlpaaxYsQIvL1nwVDyfFEXhVHSirhJiTEKabp+TjRntq7vRKdCDWl4OshZYSRF/BTaPfLBmmL0ntJ0O/u0MG5cQQggh8lTgZKxs2bLExMTg5eVF+fLl2bp1K7Vq1SI0NBRzc/MCjdWxY0e97UmTJjF//nwOHz5MlSpVmDVrFqNGjeK1114DtMmai4sLv/76K4MGDcpxzFmzZtGyZUuCg4MBCA4OZs+ePcyaNYsVK1bo+tWqVYtLly6xZs0aevXqBcCaNWvw9PTEz8+vQNchRHF3Ke4uIeHRhIRFc+lmsq7d1sKENlVd6RToTpBfaUyMpbpeiaHOhMPzYfcUyEy5t2bYEGg8QtYME0IIIUqIAidjr776Kjt27KBu3boMHTqUN954gx9//JGoqCg+/vjjJw5ErVbz+++/k5ycTFBQEBEREcTGxtKqVStdH3Nzcxo3bszBgwcfm4wdOnQoWxytW7dm1qxZ2fr279+fJUuW6JKxxYsXM2DAAHbv3p1rrOnp6aSnp+u2ExMT83mVQjw7MQmp/BmuLUX/77UH71FzEyNaVHahY4A7TSqVwcLU2IBRiicSdfjemmGntdte9aH9N7JmmBBCCFHCFDgZmzp1qu7r119/nbJly3Lw4EHKly9Pp06dChzAyZMnCQoKIi0tDRsbG9auXUuVKlU4eFBbBczFxUWvv4uLC5cvX37seLGxsTkeExsbm61v7969CQ4OJjIyEpVKxYEDB1i5cmWeydiUKVMYP358Pq9QiGfndnIGf52MISQ8mtDI29xfRdDYSEXDCk50CnCnVVVXbMxlVYsSKeU2bBsDx3/Sbls6QquJEPimdr0BIYQQQpQoT/2JrF69etSrV++Jj69UqRJhYWHEx8ezevVq+vbty549e3T7H31uRVGUPJ9lye8xTk5OtG/fnmXLlqEoCu3bt8fJySnPmIODgxk+fLhuOzExEU9PzzyPE6Io3E3PYtvpWELCotl34SZZmgfruNfxcaRToDvtqrvhaG1mwCjFU9FoIPxX2DoaUu8tH1KrD7QYD1aOho1NCCGEEE+swMnY8uXLc93fp0+fAo1nZmamK+BRu3ZtQkNDmT17Np9//jmgvdPl5uam63/jxo1sd74e5urqmu0uWG7HDBgwgCFDhgAwb968fMVsbm5e4OfjhChM6Vlqdp+LIyQsmh1nr5OW+WAtsGoedvfWAnPH3cHSgFGKQnH9NGwcDlGHtNvOVaDDTPB68j+CCSGEEKJ4KHAyNnToUL3tzMxMUlJSMDMzw8rKqsDJ2KMURSE9PR1fX19cXV3Ztm0bNWvWBCAjI4M9e/Ywbdq0xx4fFBTEtm3b9J4b27p1K/Xr18+xf5s2bXQLV7du3fqpYheiKGWpNRy6dIuQsGg2n4olKS1Lt8/PyZqOAe50CnSnXBkbA0YpCk1GMuyZBofm3VszzPremmHvS6l6IYQQ4jlR4GTszp072douXLjA+++/z2effVagsb744gvatm2Lp6cnSUlJuue1Nm/ejEqlYtiwYUyePJkKFSpQoUIFJk+ejJWVFW+++aZujD59+uDh4cGUKVMAbbLYqFEjpk2bRufOnVm/fj3bt29n//79OcZgbGzMmTNndF8LUZxkqjWERtxmy6lYNp6M5ebdB4VjXO0s6BjgRudAD6q620kp+ufJ2b9g0whIuKLd9u8AbaaCg0yHFkIIIZ4nhfIUf4UKFZg6dSpvvfUWZ8+ezfdx169fp3fv3sTExGBvb0+NGjXYvHkzLVu2BGDEiBGkpqYyePBg7ty5Q926ddm6daveGmNRUVEYGT0ox12/fn1WrlzJl19+yejRoylXrhyrVq3SW2PsUXZ2dk9w1UIUjdQMNXvOx7H1dCw7ztwgITVTt6+UlSltq7vROcCdl30cMTKSBOy5En8FNn0O5zZqt+29oN10qNTWsHEJIYQQokioFEVR8u6Wt+PHj9O4ceMXssx7YmIi9vb2JCQkSGInnsid5Ax2nL3BllOx7LsQp/cMmKO1GS0qO9O2mhuvVHDCVNYCe/6oM7XTEfdMe7BmWP0PodFnsmaYEEIIUcIUJDco8J2xkJAQvW1FUYiJiWHu3Lk0aNCgoMMJ8cK6Fp/K1lOxbD11nb8jb6N+qApi2VKWtK7qSqsqLtT2ccRY7oA9vy4f0hbouL9mmHcD7ZphzpUNG5cQQgghilyBk7EuXbrobatUKsqUKUOzZs345ptvCisuIZ47iqJw/vpdtp6KZcvpWL2FmAEqu9nRqooLrau6UtnNVp4Be94lXYedX8Hxn7XbVqW1a4YFvCFrhgkhhBAviAInYxqNJu9OQggANBqF41fusOXUdbaeiiXyVopun0oFL3s70qqqC62quOJV2sqAkYpnJiYcDi+Af/8AtbaSK7X6QotxsmaYEEII8YJ54gIeN2/exMzMTJ6REuIR6VlqDl28xZZT19l2+rpeBUQzEyMalneiVVUXmld2wclG1qt7IWjUcO4vODwfLh940F62jvZumNfjCwwJIYQQ4vlVoGQsPj6eUaNGsWrVKl2J+zJlytC/f39Gjx6NlZX8ZV+8mJLSMtl9Lo6tp6+z6+wN7qY/WAPM1sKEZv7OtK7qSqOKZbAxL5QipqIkSEuAYz/B399DfJS2zcgEqr4Kdd+Hsi8ZNj4hhBBCGFS+PxXevn2boKAgrl27Rq9evahcuTKKonDmzBm+/fZbtm3bxv79+wkPD+fIkSN89NFHRRm3EAYXl5TO9jPX2XIqloP/3SJD/WAKr7OtuW76YT2/0piZSAXEF8qti3Dkewj7BTLuatssHaF2f3j5bbBzN2x8QgghhCgW8p2MffXVV5iZmXHx4kVcXFyy7WvVqhW9e/dm69atzJkzp9ADFaI4uHwrmS33KiD+E3WHhxeG8HOyplVVV1pXdSGgrIOsAfaiURSI2KN9Huz8ZuDem6NMZaj3PtToDqaWBg1RCCGEEMVLvpOxdevW8f3332dLxABcXV2ZPn067dq1Y+zYsfTt27dQgxTCUBRF4VR0orYC4qnrnLuepLc/oKy9LgEr72z7mFHEcy0zFU7+rn0e7H55eoAKrbVJmF8TqY4ohBBCiBzlOxmLiYmhatWqj91frVo1jIyMGDt2bKEEJoShZKk1hEbeYcupWLadvs61+FTdPmMjFfX8HGld1ZWWVVxws5c7HS+sxBg4+iMcXQwpt7RtptZQsxfUGQRO5Q0bnxBCCCGKvXwnY05OTkRGRlK2bNkc90dERODs7FxogQnxLKVlqtl7XluAY8eZ69xJydTtszQ1pnHFMtoKiP4u2P+/vXuPq6rK/z/+PiAeUOEoEuAFDS9liqUoSuqYWQKlU351aqYMx0vTmOK3789pmnGqKWtmmOpbTdr9hjpT2vQdGs1KxfGW5l3wAmqWeQfvclTkvn5/bDtG4h3OPsDr+XicR+6119l81mPBI97stddpEGBjpbDdvvXWXbDsT6TyM98nrlZSz4ekrslSUGNbywMAADXHJYexpKQkPf7448rIyFD9+vUrnCsqKtKTTz6ppKSkKi8QqC7HC4q1cOtBzcvO09KvD+t0SZnnXJMGAbrtBusDmH/SPkyBAf42VgrblZVKW+dYIWzPyrPtrXpJ8WOk6wdK/uySCQAALo/DmB9uQXB+e/fuVffu3eV0OjVu3Dh16NBBkpSTk6PXX39dRUVFWrNmjVq1alWtBfsit9stl8ul/Px8PnfNx+Xmn9b87AOan5OnlTuOqqz87Ld/i8ZBnh0Q465tonr+7IBY550+Jq2fLq1+R8rfY7X5BUgxQ60Q1ryrvfUBAACfcznZ4JL/lNuyZUutWLFCY8eO1cSJE/V9hnM4HBowYIBeffXVOhnE4NuMMfrm4EnNz7G2oN+4N7/C+Q6RwUroGKGETpHq1DxEDjZagCQd3i6telPK+lAqKbDaGjSVuo+W4kZLwZH21gcAAGqFy1pXEx0drS+++ELHjh3T9u3bJUnt2rVTaGhotRQHXInycqOsvcetDTiyD2jH4VOecw6H1L11EyV0tDbguDasoY2VwqcYI3270Aph2+efbY+IsXZFjPmZFBBoX30AAKDWuaKHHJo0aaIePXpUdS3AFSsuLdeKHUc0/8wOiAdPFHnO1ff3U+92TZXYKVK33RCha4KdNlYKn1NcIG38yAphh7aeaXRI199hhbBrf8LW9AAAoFrwxDlqrJNFpVqy7ZDmZedp0daDOlFU6jkX7KynWzuEK6FThPpdH65GTr7V8SP5+6Q170jrplrPhklS/UbWjog9H5JC29haHgAAqP34DRU1yuGTRVqQc0Dzcw5o2TeHVVxa7jl3TbBTAzpaOyDGtwmVsx47IKISe9dKK1+Xsv8tmTM7aDZuLfUcY31GWKDL1vIAAEDdQRiDz9t9pEDzc/I0P/uA1u46qh9sgKhrmzZQYqdIJXSKVNeoxvLzYzkZKlFWIuXMspYi7l1ztv3an1gh7Po7JD/COwAA8C7CGHyOMUY5uW7Nz7Z2QNyad6LC+c4tXErsZO2A2D68ETsg4vwKjlrLEFe/I53Yb7X515c632OFsGY32loeAACo2whj8All5UZrdx7VvDOfAbb32GnPOX8/h3pGh3q2oG/eOMjGSlEjHNxq3QXbMFMqPfO91PAaKe5BqfsoqVG4vfUBAACIMAYbFZaUadn2w5qfk6cFWw7q6Kliz7nAAD/1bX+NEjtFqn+HcDVpWN/GSlEjlJdL3/5HWvmG9d/vRd4oxY+VYoZI9dhJEwAA+A7CGLwq/3SJFm09qHnZeVry9SEVFJd5zjVuEKDbOkQooVOE+ra/RkH1eYYHl6D4lLRhhrTyTemI9fmHcvhJHQZKPR+WWvdia3oAAOCTCGOodnn5hcrIydP8nANa8e0Rlf5gB47mrkAldIpUQqcI9bg2VPX8/WysFDXK8T3S6rel9dOkwnyrzRkixQ6XevxKanKtreUBAABcDGEM1eKbgyc1PydP87IPaMOe4xXOXRfRyNoBsWOkYlqEsAEHLp0x0p7V1tb0Wz49uzV9k2jrA5q73C85g+2tEQAA4BIRxlAlysuNNu7L1/zsPM3LztO3h055zjkcUmyrJp4NOKLDGtpYKWqk0mIp59/W82D7159tj77Feh6sfYLkx11VAABQsxDGcMVKysq1asdRzcvOU0bOAeW5Cz3nAvwd6tU2TImdInV7x3CFBwfaWClqrFNHpHXvS6vflU7mWW3+TunGe607YRGd7K0PAADgKhDGcFkKiku1ZNshzc85oP9sOSB3YannXCNnPfW7/holdIrUrddfo+DAABsrRY12IEda9Ya08Z9S6ZmQ3yjyzNb0I6WGYfbWBwAAUAUIY7ioo6eKtWDLAc3PztOX2w+rqLTccy6sUX0NOLP8sFfbpnLWYwdEXKHycmn7fOt5sO+WnG1v3tVaithxsFSPjzgAAAC1B2EMldpztEDzc6wAtmbnUf1gA0S1btrgzAYcEeraqon8/diAA1eh6ISUNcO6E3Z0h9Xm8JNuuMtaihjVk63pAQBArUQYgyTJGKOteSc0P/uA5mXnKSfXXeF8TIsQJXSMVGKnSF0X0YgdEHH1ju2UVr8jrZ8uFZ35fgt0SbG/tLamb9zK1vIAAACqG2GsDisrN1q/+5jmbbY+A2z30QLPOT+H1CM6VImdIjWgY4RaNmlgY6WoNYyRdq+wliJu/UwyZ5a8Nm0n9Rwj3XSf5Gxkb40AAABeQhirYwpLyvTVt4c1P/uAFmw5oMMniz3nnPX89JP21yixU4RuuyFCoQ15PgdVpLRI2pxuLUXM3XC2vW1/63mwtrexNT0AAKhzbP3tJzU1VXFxcQoODlZ4eLgGDx6sbdu2VejjcDgqfb3wwgvnve7UqVMrfU9h4dmt10eMGCGHw6ExY8ac8/6xY8fK4XBoxIgRVTZWO7kLSzQra5/GfbBe3Z7N0KipazVzzR4dPlmskMB6GtK1hd58IFaZfxygd3/ZXfd0jyKIoWqcPCQtfk56OUb69xgriNULlLqNkMaulJI/kdoPIIgBAIA6ydY7Y0uWLNG4ceMUFxen0tJSPf7440pISFBOTo4aNrQ+GDg3N7fCe7744guNHj1aQ4cOveC1Q0JCzgl2gYEVP+sqKipKM2fO1Msvv6ygoCBJUmFhoWbMmKFWrWr28yoH3YXWBhw5B7Ti28MqKTu7A0dkSKASOkUosVOkekSHKsCfX4RRxXI3SqvelDZ9LJWdufsa3Nx6FqzbCKlBqK3lAQAA+AJbw9jcuXMrHKelpSk8PFzr1q1T3759JUmRkZEV+syaNUu33nqr2rRpc8FrOxyOc977Y7GxsdqxY4fS09M1bNgwSVJ6erqioqIuen1f9fHaPfpw9W5l7j5eob1deCMldopQQsdI3djSxQYcqHrlZdLXc6WVb0g7vzzb3qK7tStix7slfz57DgAA4Hs+9cxYfn6+JCk0tPK/mh84cECfffaZpk2bdtFrnTx5Uq1bt1ZZWZm6dOmiZ599Vl27dj2n38iRI5WWluYJY++//75GjRqlxYsXn/faRUVFKioq8hy73e7z9vW2bw+d8gSxrq0aK6FjpBI6RajtNWyKgGpS6JYy/yGtfsvaIVGSHP5Sp8FSz4elqDg7qwMAAPBZPhPGjDGaMGGC+vTpo5iYmEr7TJs2TcHBwRoyZMgFr9WhQwdNnTpVnTt3ltvt1iuvvKLevXtrw4YNat++fYW+ycnJmjhxonbu3CmHw6Hly5dr5syZFwxjqampmjRp0mWP0RuGxLZQyyZBGtAxQhEhgRd/A3Clju6QVr1tBbHiE1ZbYGOp+0gp7kHJ1dLW8gAAAHydz4SxlJQUbdy4UcuWLTtvn/fff1/Dhg0759mvH4uPj1d8fLznuHfv3oqNjdWUKVM0efLkCn3DwsI0cOBATZs2TcYYDRw4UGFhYRe8/sSJEzVhwgTPsdvtVlRU1AXf4y3XRQTruohgu8tAbWWMtHOZtRRx2+eSzjyLGHa9FD9GuvEXUn0+BgEAAOBS+EQYGz9+vGbPnq2lS5eqZcvK/5r+5Zdfatu2bfroo48u+/p+fn6Ki4vT9u3bKz0/atQopaSkSJJee+21i17P6XTK6XRedh1AjVVSKG3+PyuEHdh8tr3dAOt5sLb9JZ5DBAAAuCy2hjFjjMaPH69PPvlEixcvVnR09Hn7vvfee+rWrZtuuummK/o6WVlZ6ty5c6Xnk5KSVFxs7fiWmJh42dcHaq0TB6S170lr3pMKDlttAQ2sD2fuOUa65jp76wMAAKjBbA1j48aN04cffqhZs2YpODhYeXl5kiSXy+XZal6ylgF+/PHHevHFFyu9zvDhw9WiRQulpqZKkiZNmqT4+Hi1b99ebrdbkydPVlZW1nnvevn7+2vLli2efwN13v4s6y7Y5n9J5SVWW0hLqedDUuxwKaiJreUBAADUBraGsTfeeEOS1K9fvwrtaWlpFT5weebMmTLG6L777qv0Ort375bfDz409vjx43rooYeUl5cnl8ulrl27aunSperRo8d5awkJCbnygQC1QXmZtPUzK4Tt/upse1RPaylih59K/j6xshkAAKBWcBhjzMW74ULcbrdcLpfy8/MJdah5Th+XMv8urX5bOr7bavOrJ3UaYm3K0aKbreUBAADUJJeTDfgzN1BXHflWWvWmlPmBVHLKagsKlbqPsramD2lmb30AAAC1HGEMqEuMkXYstpYibp93tj28o7Uhx433SgFB5307AAAAqg5hDKgLSk5LG/9phbBDW862X5dkPQ8WfQtb0wMAAHgZYQyozdz7pTXvSmvTpNNHrbaAhlLXYVKPX0th7eytDwAAoA4jjAG10b511l2w7E+k8lKrzdVK6vlrqesDUlBjW8sDAAAAYQyoPcpKpa2fWiFsz6qz7a16WUsRr7+TrekBAAB8CL+ZATVdwVFp/XRp9TuSe6/V5hcgdf6ZtSlH8y62lgcAAIDKEcaAmurQ19bW9BtmSCUFVluDMClutNR9tBQcYW99AAAAuCDCGFCTGCN9+x9rKeI3C862R8RYSxFjfiYFBNpXHwAAAC4ZYQyoCYoLpI0zpZVvSoe3nWl0WM+BxT8sXduHrekBAABqGMIY4MvKy6UNH0oLnpZOHbLa6gdLsclSj19JoW1sLQ8AAABXjjAG+Kr9WdLnj0p711jHjVtbd8G6DJMCQ2wtDQAAAFePMAb4moKj0sI/SWvfl2SsD2nu9zup58NSvfp2VwcAAIAqQhgDfEV5uZT5d+k/k6SCI1ZbzFAp4U9SSHN7awMAAECVI4wBvmDfemtJ4r511vE1HaQ7X5Ci+9pbFwAAAKoNYQywU8FR6T/PSOumSjLW5hz9fi/1/LXkH2B3dQAAAKhGhDHADuVl0vrp1pLE08ests73SgnPSsGR9tYGAAAAryCMAd62d530+W+k/ZnWcXhHa0nitX3srQsAAABeRRgDvOXUEek/T0vr/y7JSM4Qqd9E6/PCWJIIAABQ5xDGgOpWXmY9E/afZ6TC41bbjb+QBjwjBUfYWRkAAABsRBgDqtOeNdaSxNwN1nFEjHTn/0qtb7a3LgAAANiOMAZUh1OHpQVPSZn/sI6dIVL/J6TuoyV/fuwAAABAGAOqVnmZtPZ9aeGzUmG+1XbT/dKASVKjcHtrAwAAgE8hjAFVZfcqa0li3ibrOLKzdOeLUque9tYFAAAAn0QYA67WyUPWksSsD6zjQJfU/0mp+yjJz9/e2gAAAOCzCGPAlSorlda+Jy38s1R0Zkli1wek256WGl1ja2kAAADwfYQx4ErsWiF9/qh0YLN13Owma0liVJy9dQEAAKDGIIwBl+PEASnjj9LGmdZxYGPptielbiNZkggAAIDLQhgDLkVZqbT6bWlxqlTkluSQYpOtJYkNm9pdHQAAAGogwhhwMTuXS5//VjqYbR0372otSWzZzd66AAAAUKMRxoDzOZEnzX9S2vRP6zioiXTbU1LscJYkAgAA4KoRxoAfKyuxliQuSpWKT0hySN1GSLf9UWoQand1AAAAqCUIY8AP7VwmffaodGiLddyim3Tn/0otYu2tCwAAALWOn51fPDU1VXFxcQoODlZ4eLgGDx6sbdu2VegzYsQIORyOCq/4+PiLXvtf//qXOnbsKKfTqY4dO+qTTz6p9Lpjxow5571jx46Vw+HQiBEjrmp8qEHcudL/jZamDrSCWFCo9NPJ0ugFBDEAAABUC1vD2JIlSzRu3DitXLlSGRkZKi0tVUJCgk6dOlWhX1JSknJzcz2vzz///ILXXbFihX7+858rOTlZGzZsUHJysu69916tWrWqQr+oqCjNnDlTp0+f9rQVFhZqxowZatWqVdUNFL6rrERaPll6tbu0+f8kOaTuo6Xx66Ruv5T8bP0RAQAAQC1m6zLFuXPnVjhOS0tTeHi41q1bp759+3ranU6nIiMjL/m6f/vb3zRgwABNnDhRkjRx4kQtWbJEf/vb3zRjxgxPv9jYWO3YsUPp6ekaNmyYJCk9PV1RUVFq06bN1QwNNcGOJdYuiYfP3I1tGWctSWzexdayAAAAUDf41J/98/PzJUmhoRU3SVi8eLHCw8N13XXX6Ve/+pUOHjx4weusWLFCCQkJFdoSExP11VdfndN35MiRSktL8xy///77GjVq1AWvX1RUJLfbXeGFGiR/n/TxSGn6XVYQa9BUuvs1adR8ghgAAAC8xmfCmDFGEyZMUJ8+fRQTE+Npv+OOO/TBBx9o4cKFevHFF7VmzRr1799fRUVF571WXl6eIiIiKrRFREQoLy/vnL7JyclatmyZdu7cqV27dmn58uV64IEHLlhramqqXC6X5xUVFXWZo4UtSoulZX+TXo2TstMlh58U9ytrSWLXB1iSCAAAAK/ymd0UU1JStHHjRi1btqxC+89//nPPv2NiYtS9e3e1bt1an332mYYMGXLe6zkcjgrHxphz2iQpLCxMAwcO1LRp02SM0cCBAxUWFnbBWidOnKgJEyZ4jt1uN4HM1327SPriMenw19ZxVE9rSWKzG+2tCwAAAHWWT4Sx8ePHa/bs2Vq6dKlatmx5wb7NmjVT69attX379vP2iYyMPOcu2MGDB8+5W/a9UaNGKSUlRZL02muvXbRep9Mpp9N50X7wAfl7pXl/kHJmWccNr5EGPCPd+AvuhAEAAMBWtv42aoxRSkqK0tPTtXDhQkVHR1/0PUeOHNGePXvUrFmz8/a5+eablZGRUaFt/vz56tWrV6X9k5KSVFxcrOLiYiUmJl7eIOCbSoukL1+0liTmzLKWJPYcI6WslbrcTxADAACA7Wy9MzZu3Dh9+OGHmjVrloKDgz13s1wul4KCgnTy5Ek9/fTTGjp0qJo1a6adO3fqD3/4g8LCwvRf//VfnusMHz5cLVq0UGpqqiTpkUceUd++ffXcc8/p7rvv1qxZs7RgwYJzlkB+z9/fX1u2bPH8GzXcN/+xliQe+cY6bnWzdOcLUmRne+sCAAAAfsDWMPbGG29Ikvr161ehPS0tTSNGjJC/v782bdqk6dOn6/jx42rWrJluvfVWffTRRwoODvb03717t/x+cKejV69emjlzpp544gk9+eSTatu2rT766CP17NnzvLWEhIRU7eDgfcf3SPMmSls+tY4bhksJz0o3/lyq5HlBAAAAwE4OY4yxu4iazu12y+VyKT8/n1Bnh9Ii6avJ0tIXpdLTksNf6vlrqd/vpUCX3dUBAACgDrmcbOATG3gAV2x7hrUk8egO67h1b2tJYkQne+sCAAAALoIwhprp2C5p7kRp22fWcaNIKeFPUuefsSQRAAAANQJhDDVLSaG1JPHLF6XSQmtJYvzD0i2/kwJZIgoAAICagzCGmuPredIXv5OOfWcdX/sTa0li+A321gUAAABcAcIYfN/R76wliV9/YR0HN7OWJMYMZUkiAAAAaizCGHxXyWlp2d+kZS9LZUWSXz0pfqx0y2OSM/iibwcAAAB8GWEMvmnbF9aSxOO7rOPoW6wliddcb29dAAAAQBUhjMG3HN0hffF7afs86zi4uZT0F6njYJYkAgAAoFYhjME3FBdYyxGXv3JmSWKAdPM4qe9vJWcju6sDAAAAqhxhDPYyRtr6mbVBR/5uq63NrdaSxLD29tYGAAAAVCPCGOxz5Fvpi8ekbxZYxyEtrSWJN9zFkkQAAADUeoQxeF/xKetDm7+aIpUVS/71pV7jpZ/8Rqrf0O7qAAAAAK8gjMF7jJG2fCrN+4OUv8dqa3ubdMfzUlg7e2sDAAAAvIwwBu84/I30xW+lbxdax64oKSlV6jCIJYkAAACokwhjqF7Fp6SlL0hfvSqVl1hLEns/IvWZINVvYHd1AAAAgG0IY6gexkg5/5bmPS6591lt7ROkpL9KTdvaWhoAAADgCwhjqHqHvraWJO5YbB03biUlPSddfwdLEgEAAIAzCGOoOkUnpaXPSyteP7Mk0Sn1+R+pz/+TAoLsrg4AAADwKYQxXD1jpOx0ad4T0on9Vtt1SdYGHaFt7K0NAAAA8FGEMVydg1utJYnfLbWOm1x7Zklikq1lAQAAAL6OMIYrU3RCWvKctPINqbxUqhdo7ZDY+xEpINDu6gAAAACfRxjD5TFG2vwvaf4T0olcq+36gVLSX6y7YgAAAAAuCWEMl+7gFunz30o7v7SOm0RLdzwvXZdgb10AAABADUQYw8UVus8uSTRlUr0g6Se/kXqNZ0kiAAAAcIUIYzg/Y6RNH1tLEk8esNo6DLJ2SWzcyt7aAAAAgBqOMIbKHci2liTuWm4dh7aR7nhBan+7vXUBAAAAtQRhDBUV5kuLUqXVb59dktj3UWtJYj2n3dUBAAAAtQZhDBZjpA0zpYw/SqcOWm033CUl/kVqHGVvbQAAAEAtRBiDlLdJ+uxRac9K67hpO2uXxHa32VsXAAAAUIsRxuqy08elRX+R1rwjmXIpoIHU97fSzeNYkggAAABUM8JYXVReLm2YIS14Sjp1yGrrOFhK/LPkamlraQAAAEBdQRira3I3WEsS9662jsOus5Yktr3V3roAAACAOsbPzi+empqquLg4BQcHKzw8XIMHD9a2bds850tKSvS73/1OnTt3VsOGDdW8eXMNHz5c+/fvv+B1p06dKofDcc6rsLDQ02fEiBFyOBwaM2bMOe8fO3asHA6HRowYUWVjtd3pY9Jnv5He7mcFsYCG0oBnpDHLCWIAAACADWwNY0uWLNG4ceO0cuVKZWRkqLS0VAkJCTp16pQkqaCgQOvXr9eTTz6p9evXKz09XV9//bXuuuuui147JCREubm5FV6BgYEV+kRFRWnmzJk6ffq0p62wsFAzZsxQq1a15EONy8ul9X+XpnST1rxrPRsWM1Qav1bq/YhUr77dFQIAAAB1kq3LFOfOnVvhOC0tTeHh4Vq3bp369u0rl8uljIyMCn2mTJmiHj16aPfu3RcMTA6HQ5GRkRf8+rGxsdqxY4fS09M1bNgwSVJ6erqioqLUpk2bKxyVD9mfaS1J3LfWOr6mg3TnC1J0X3vrAgAAAGDvnbEfy8/PlySFhoZesI/D4VDjxo0veK2TJ0+qdevWatmypQYNGqTMzMxK+40cOVJpaWme4/fff1+jRo264LWLiorkdrsrvHxKwVFpzv+T3r7VCmL1G0kJf5LGLCOIAQAAAD7CZ8KYMUYTJkxQnz59FBMTU2mfwsJC/f73v9f999+vkJCQ816rQ4cOmjp1qmbPnq0ZM2YoMDBQvXv31vbt28/pm5ycrGXLlmnnzp3atWuXli9frgceeOCCtaampsrlcnleUVE+9KHImf+wliSufV+SkTrfI6WslXqNl/wD7K4OAAAAwBk+s5tiSkqKNm7cqGXLllV6vqSkRL/4xS9UXl6u119//YLXio+PV3x8vOe4d+/eio2N1ZQpUzR58uQKfcPCwjRw4EBNmzZNxhgNHDhQYWFhF7z+xIkTNWHCBM+x2+32nUB2cIt0+qgU3tFaknhtH7srAgAAAFAJnwhj48eP1+zZs7V06VK1bHnu51yVlJTo3nvv1XfffaeFCxde8K5YZfz8/BQXF1fpnTFJGjVqlFJSUiRJr7322kWv53Q65XT66Ici9/u91ORaqdsI7oQBAAAAPszWZYrGGKWkpCg9PV0LFy5UdHT0OX2+D2Lbt2/XggUL1LRp0yv6OllZWWrWrFml55OSklRcXKzi4mIlJiZe9vV9ijNY6vErghgAAADg42y9MzZu3Dh9+OGHmjVrloKDg5WXlydJcrlcCgoKUmlpqX72s59p/fr1mjNnjsrKyjx9QkNDVb++tS378OHD1aJFC6WmpkqSJk2apPj4eLVv315ut1uTJ09WVlbWee96+fv7a8uWLZ5/AwAAAEB1szWMvfHGG5Kkfv36VWhPS0vTiBEjtHfvXs2ePVuS1KVLlwp9Fi1a5Hnf7t275ed39ibf8ePH9dBDDykvL08ul0tdu3bV0qVL1aNHj/PWcrlLHwEAAADgajiMMcbuImo6t9stl8ul/Px8Qh0AAABQh11ONvCZre0BAAAAoC4hjAEAAACADQhjAAAAAGADwhgAAAAA2IAwBgAAAAA2IIwBAAAAgA0IYwAAAABgA8IYAAAAANiAMAYAAAAANiCMAQAAAIANCGMAAAAAYIN6dhdQGxhjJElut9vmSgAAAADY6ftM8H1GuBDCWBU4ceKEJCkqKsrmSgAAAAD4ghMnTsjlcl2wj8NcSmTDBZWXl2v//v0KDg6Ww+Gwuxy53W5FRUVpz549CgkJsbscVAHmtPZhTmsn5rX2YU5rJ+a19vGlOTXG6MSJE2revLn8/C78VBh3xqqAn5+fWrZsaXcZ5wgJCbH9mxFVizmtfZjT2ol5rX2Y09qJea19fGVOL3ZH7Hts4AEAAAAANiCMAQAAAIANCGO1kNPp1FNPPSWn02l3KagizGntw5zWTsxr7cOc1k7Ma+1TU+eUDTwAAAAAwAbcGQMAAAAAGxDGAAAAAMAGhDEAAAAAsAFhDAAAAABsQBjzQampqYqLi1NwcLDCw8M1ePBgbdu2rUIfY4yefvppNW/eXEFBQerXr5+ys7Mr9CkqKtL48eMVFhamhg0b6q677tLevXvP+XqfffaZevbsqaCgIIWFhWnIkCHVOr66yFtzunjxYjkcjkpfa9as8cpY6xJv/qx+/fXXuvvuuxUWFqaQkBD17t1bixYtqvYx1jXenNP169drwIABaty4sZo2baqHHnpIJ0+erPYx1jVVNadvv/22+vXrp5CQEDkcDh0/fvycr3Xs2DElJyfL5XLJ5XIpOTm50n64et6c1z//+c/q1auXGjRooMaNG1fjqOo2b83pzp07NXr0aEVHRysoKEht27bVU089peLi4uoeYqUIYz5oyZIlGjdunFauXKmMjAyVlpYqISFBp06d8vR5/vnn9dJLL+nVV1/VmjVrFBkZqQEDBujEiROePv/zP/+jTz75RDNnztSyZct08uRJDRo0SGVlZZ4+//rXv5ScnKyRI0dqw4YNWr58ue6//36vjrcu8Nac9urVS7m5uRVeDz74oK699lp1797d6+Ou7bz5szpw4ECVlpZq4cKFWrdunbp06aJBgwYpLy/Pq2Ou7bw1p/v379ftt9+udu3aadWqVZo7d66ys7M1YsQIbw+51quqOS0oKFBSUpL+8Ic/nPdr3X///crKytLcuXM1d+5cZWVlKTk5uVrHV1d5c16Li4t1zz336OGHH67WMdV13prTrVu3qry8XG+99Zays7P18ssv680337zg90C1MvB5Bw8eNJLMkiVLjDHGlJeXm8jISPPXv/7V06ewsNC4XC7z5ptvGmOMOX78uAkICDAzZ8709Nm3b5/x8/Mzc+fONcYYU1JSYlq0aGHeffddL44GxlTfnP5YcXGxCQ8PN88880w1jgbfq655PXTokJFkli5d6unjdruNJLNgwQJvDK3Oqq45feutt0x4eLgpKyvz9MnMzDSSzPbt270xtDrrSub0hxYtWmQkmWPHjlVoz8nJMZLMypUrPW0rVqwwkszWrVurZzDwqK55/aG0tDTjcrmqunSchzfm9HvPP/+8iY6OrrLaLwd3xmqA/Px8SVJoaKgk6bvvvlNeXp4SEhI8fZxOp2655RZ99dVXkqR169appKSkQp/mzZsrJibG02f9+vXat2+f/Pz81LVrVzVr1kx33HHHObd7UfWqa05/bPbs2Tp8+DB/bfeS6prXpk2b6oYbbtD06dN16tQplZaW6q233lJERIS6devmreHVSdU1p0VFRapfv778/M7+bzgoKEiStGzZsuodVB13JXN6KVasWCGXy6WePXt62uLj4+VyuS7rOrgy1TWvsI835zQ/P9/zdbyNMObjjDGaMGGC+vTpo5iYGEnyLEuKiIio0DciIsJzLi8vT/Xr11eTJk3O22fHjh2SpKefflpPPPGE5syZoyZNmuiWW27R0aNHq3VcdVl1zumPvffee0pMTFRUVFRVDwM/Up3z6nA4lJGR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)</div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [ ]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span> 
```

</div> </div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [ ]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span> 
```

</div> </div></div></div></div></div><div class="jp-Cell jp-CodeCell jp-Notebook-cell jp-mod-noOutputs  "><div class="jp-Cell-inputWrapper"><div class="jp-Collapser jp-InputCollapser jp-Cell-inputCollapser"></div><div class="jp-InputArea jp-Cell-inputArea"><div class="jp-InputPrompt jp-InputArea-prompt">In [ ]:</div><div class="jp-CodeMirrorEditor jp-Editor jp-InputArea-editor" data-type="inline"><div class="CodeMirror cm-s-jupyter"><div class=" highlight hl-ipython3">```
<span></span> 
```

</div> </div></div></div></div></div>