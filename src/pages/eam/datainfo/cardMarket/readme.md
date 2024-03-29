# CARDMARKET

首先，调研了实现前端托拽组件的库，发现了 SortableJS 作者有相关的库，并且提供了 react 的封装，地址如下：

- Sortable: https://github.com/SortableJS/Sortable
- react-sortablejs: https://github.com/SortableJS/react-sortablejs

使用 react-sortablejs 库进行了测试，代码如下，使用该方式可以实现将工具栏中的组件托拽到工作区中。

```tsx
import { PageContainer } from '@ant-design/pro-components';
import { Button } from 'antd';
import { useState } from 'react';
import { ReactSortable } from 'react-sortablejs';

const CardMarketView: React.FC = () => {
  const [usePic, setUsePic] = useState<any[]>([
    { id: 1, name: 'bar1', isPrototype: false },
    { id: 2, name: 'bar2', isPrototype: false },
  ]);

  const [diagram, setDiagram] = useState<any[]>([
    { id: 1, name: '折线图', isPrototype: true },
    { id: 2, name: '柱状图', isPrototype: true },
    { id: 3, name: '扇形图', isPrototype: true },
  ]);

  return (
    <PageContainer
      fixedHeader={false}
      subTitle={
        <>
          <Button
            type="primary"
            size="small"
            onClick={() => {
              console.log('---1');
            }}
          >
            更新
          </Button>
        </>
      }
    >
      {/* 工作区 */}
      <div style={{ display: 'flex', width: '100%', height: '60vh', backgroundColor: '#ffccc7' }}>
        <ReactSortable
          group={{
            name: 'shared',
          }}
          list={usePic}
          setList={(pics) => {
            const newState = pics.map((pic) => {
              let tmp = pic;
              let tmpMaxIndex = Math.max(...usePic.map((item) => item.id));

              if (pic.isPrototype) {
                tmp = { ...pic, id: tmpMaxIndex + 1, isPrototype: false };
              }
              return tmp;
            });

            setUsePic(newState);
          }}
          style={{
            display: 'flex',
            flexDirection: 'row',
            flexWrap: 'wrap',
          }}
        >
          {usePic.map((item) => (
            <div
              style={{
                width: '100px',
                height: '100px',
                backgroundColor: '#f0f0f0',
                margin: '10px',
              }}
              key={item.id}
              onClick={() => {
                console.log('---2');
              }}
            >
              {item.name}
            </div>
          ))}
        </ReactSortable>
      </div>

      {/* 工具栏 */}
      <div
        style={{
          position: 'absolute',
          bottom: 0,
          background: '#595959',
          width: '100%',
          height: '100px',
        }}
      >
        <ReactSortable
          group={{ name: 'shared', put: false, pull: 'clone' }}
          list={diagram}
          setList={setDiagram}
          sort={false}
          style={{ display: 'flex', padding: '0 3px' }}
        >
          {diagram.map((item) => (
            <div
              style={{
                width: '120px',
                height: '100px',
                backgroundColor: '#f6ffed',
                margin: '0 3px',
              }}
              key={item.id}
            >
              {item.name}
            </div>
          ))}
        </ReactSortable>
      </div>
    </PageContainer>
  );
};

export default CardMarketView;
```

工作台一期问题

1. 页面布局设置应该考虑 px 和 %
