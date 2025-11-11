import { TransformWrapper, TransformComponent } from "react-zoom-pan-pinch";

# Introduction

<TransformWrapper
  defaultScale={1}
  defaultPositionX={0}
  defaultPositionY={0}
  wheel={{ step: 0.1 }}
  doubleClick={{ disabled: true }}
>
  <TransformComponent>
    <img
      src="/img/db-schema.svg"
      alt="Database Schema"
      style={{ width: "100%", height: "auto", display: "block" }}
    />
  </TransformComponent>
</TransformWrapper>
(Scroll to Zoom, Pinch to Move Around)