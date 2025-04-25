# マルコフ決定過程  
エージェントが環境の中で行動を選び、報酬を得ながら最適な戦略を学ぶモデル  
未来は現在の状態と行動だけで決まる(マルコフ性)  
「累積報酬が最大になるような戦略(方策)」を見つけるのがゴール　

$$　
p(s', r \mid s, a) = P(S_{t+1} = s', R_{t+1} = r \mid S_t = s, A_t = a)
$$<br>　　

$$
p(s', r \mid s, a)
$$<br>
→状態sで行動aをとったとき、次の状態がs'になり、報酬がrになる確率

```
#1.遷移確率の定義
transition_probabilities = {
    #キーは(現在の状態,現在の行動)
    #値はさらに別の辞書{(次の状態,報酬):確率}
    #例：「state1でaction1を取ったら、80%の確率でstate2&報酬5、20%の確率でstate3&報酬2」
    ('state1', 'action1'): {('state2', 5): 0.8, ('state3', 2): 0.2},
    ('state1', 'action2'): {('state1', 0): 1.0},
    ('state2', 'action1'): {('state2', 3): 0.5, ('state3', 1): 0.5},
    # 他にも定義できる
}

#２．関数pの定義
#「状態sで行動aを取ったとき、次の状態がs'で報酬rになる確率」を返す
#引数は4つ：s_next　→　次の状態 𝑠′,　r　→　報酬r　,　s　→　現在の状態s　,　a　→　現在の行動a
 
def p(s_next, r, s, a):
    
    #３．遷移辞書から取り出す
    #次の状態と報酬のリストを取り出す
    outcomes = transition_probabilities.get((s, a), {})
    #4. 特定の結果の確率を取り出す
    #(s_next, r) に対応する確率を取り出す
    return outcomes.get((s_next, r), 0.0)

# ５．使用例
#「state1でaction1したとき、state2に移動して報酬5を得る確率は？」→ 0.8
print(p('state2', 5, 'state1', 'action1'))  
#「state1でaction1したとき、state3に移動して報酬2を得る確率は？」→ 0.2
print(p('state3', 2, 'state1', 'action1'))  
#「state1でaction1したとき、state3に移動して報酬5を得る確率は？」→そんなパターンないので 0.0
print(p('state3', 5, 'state1', 'action1'))
```
