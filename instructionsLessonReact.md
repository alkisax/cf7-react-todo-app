**κάνω μια διαδικάσία οπου συνδέω τον φάκελό μου με δύο git repo, το δικό μου και του καθηγητή ωστε να έχω ένα branch στο οποίο θα κάνω pull τον κόδικα του μαθήματος αλλα θα προσθέτω και δικά μου σχόλια**
```bash
git remote add origin git@github.com:alkisax/cf7-react-lessons.git   # Your personal repo
git remote add teacher https://github.com/DamMarin/cf7-react-intro.git  # Teacher's repo
git checkout -b notes
git fetch teacher
git merge teacher/main
git merge teacher/main --allow-unrelated-histories
# εδω μπήκα σε κατάσταση merge conflict και αφού τα έλυσα προσθεσα το συγκεκριμένο αρχείο (εδώ .gitignore)
git add .gitignore
git commit -m "Merge teacher/main into notes with resolved conflict"
# επιστρέφω στο Main
git checkout main
git merge notes
```
αυτο έκανα για να πάρω το version
```bash
git checkout main
git checkout tags/2025.05.26 -- .
git add .
git commit -m "Update project to lesson 2025.05.26 from teacher repo"
```

npm create vite@latest my-name -- --template react-ts 
npm install

**20/5/2025**  

npm run build  
npm run preview  
npm run lintls  

φάκελοι: assets/components/hooks/pages/utils

# πρώτο component

https://tailwindcss.com/docs/installation/using-vite
`npm install tailwindcss @tailwindcss/vite`
-> vite.config.ts για προσθήκη του tailwind οπως τα παίρνω απο το site
`import tailwindcss from '@tailwindcss/vite'`
-> app.css
`@import "tailwindcss";`

- pages/ViteIntro.tsx
τα έκανα κοπι πειστ απο το app για να τα έχω σε χωριστά αρχεία
- - `export default ViteIntro` συμαντικό
```tsx
import { useState } from 'react'
import reactLogo from '../assets/react.svg'  //αυτά στην μετακόμιση μπορεί να χρειάζονται αλλαγή
import viteLogo from '/vite.svg'
import '../App.css'

function ViteIntro() {
  const [count, setCount] = useState(0)

  return (
    <>
      <div>
        <a href="https://vite.dev" target="_blank">
          <img src={viteLogo} className="logo" alt="Vite logo" />
        </a>
        <a href="https://react.dev" target="_blank">
          <img src={reactLogo} className="logo react" alt="React logo" />
        </a>
      </div>
      <h1>Vite + React</h1>
      <div className="card">
        <button onClick={() => setCount((count) => count + 1)}>
          count is {count}
        </button>
        <p>
          Edit <code>src/App.tsx</code> and save to test HMR
        </p>
      </div>
      <p className="read-the-docs text-center text-blue-400">
        Click on the Vite and React logos to learn more
      </p>
    </>
  )
}

export default ViteIntro
```

- οπότε έχω ένα καθαρό app.tsx
```tsx
import ViteIntro from "./pages/ViteIntro.tsx";

function App() {

  return (
    <>
      <ViteIntro />
    </>
  )
}

export default App
```

### class components - Πια Obsolete

#### ClassComponent.tsx
- {} για να περάσω στην html μεταβλητή
- προσοχή στο export default
```tsx
import {Component} from "react";

class ClassComponent extends Component{
  render(){

    const title = "Is a Class Component"

    return <h1 className="text-center text-xl font-bold mt-12">{title}</h1>
  }
}

export default ClassComponent;
```

- app.tsx
```tsx
import ClassComponent from "./components/ClassComponent.tsx";

<>
  <ClassComponent/>
</>
```

### functional component
#### FunctionalComponent.tsx
- title: string  βαλαμε types γιατι είμαστε σε ts
```tsx
function FunctionalComponent(){
const title: string = "Is a Functional Component!";
return <h1 className="text-center text-xl font-bold mt-12">{title}</h1>;
}

export default FunctionalComponent;
```

- app.tsx
```tsx
import FunctionalComponent from "./components/FunctionalComponent.tsx";

<>
      <FunctionalComponent/>
</>
```

### ArrowFunctionalComponent
#### ArrowFunctionalComponent.tsx
```tsx
const ArrowFunctionalComponent = () => {
const title: string = "Is a Arrow Functional Component";
return <h1 className="text-center text-xl font-bold mt-12">{title}</h1>;
}

export default ArrowFunctionalComponent;
```
- app.tsx
```tsx
import ArrowFunctionalComponent from "./components/ArrowFunctionalComponent.tsx";

<>
  <ArrowFunctionalComponent/>
</>
```

# props
πως περνάμε πληροφορία απο το ένα component στο άλλο
- δύο συμαντικές λέξεις για ts, type και interface. Το interface είναι πιο κοινό σε OOP

#### ArrowFunctionalComponentWithProps.tsx
- είναι συμαντικό στα props να δηλώνετε ο τυπος 
```tsx
type Props = {
  title: string;
}
```
- `({title}: Props) => {`
```tsx
type Props = {
  title: string;
}

const ArrowFunctionalComponentWithProps = ({title}: Props) => {
return <h1 className="text-center text-xl font-bold mt-12">{title}</h1>;
}

export default ArrowFunctionalComponentWithProps;
```

- App.tsx
- - εδώ περάσαμε και επιπλέων πληροφορίες στο <... title="" />, αν και συχνά <... title={} />
```tsx
import ArrowFunctionalComponentWithProps from "./components/ArrowFunctionalComponentWithProps.tsx";

<>
  <ArrowFunctionalComponentWithProps title="Is a Arrow Functional Component with Props!"/>
</>
```

### δύο συμαντικές λέξεις για ts, type και interface. Το interface είναι πιο κοινό σε OOP

#### ArrowFunctionalComponentWithPropsType.tsx
- `type Props = A & B;` στην περίπτωση του interface όμως αυτό δεν χρειάστικε γιατι τα προσθέτει κατευθείαν
- `({title, description}: Props) `
```tsx
// type Props = {
//   title: string;
//   description: string;
// }

type A = {
  title: string;
}
type B ={
  description: string;
}
type Props = A & B;

// interface Props {
//   title: string;
// }
//
// interface Props {
//   description: string;
// }

const ArrowFunctionalComponentWithPropsType = ({title, description}: Props) => {
  return (
    <>
      <h1 className="text-center text-xl font-bold mt-12">{title}</h1>;
      <p className="text-center text-gray-800">{description}</p>
    </>
  )
}

export default ArrowFunctionalComponentWithPropsType;
```

- App.tsx
```tsx
import ArrowFunctionalComponentWithPropsType from "./components/ArrowFunctionalComponentWithPropsType.tsx";

<>
  <ArrowFunctionalComponentWithPropsType
    title="Is a Arrow Functional Component with Props!"
    description="this is a description"
  />
</>
```

**21/5/2025**
# component composition
#### CodingFactoryLogo.tsx
- δεν έχει τίποτα εκτώς απο την μορφοποίηση της εικόνας 
- νομιζα την εικόνα πρέπει να την κάνεις Import...
- ύψος rem 16, margin y 4
- το πέρασα ως `/logo.png` και οχι με ./ γιατί είναι στο Public
```tsx
const CodingFactoryLogo = () => {
  return (
    <>
      <img
        className="my-4 h-16"
        src="/logo.png"
        alt="CF7"
      />
    </>
  )
}

export default CodingFactoryLogo;
```
- App.tsx
```tsx
import CodingFactoryLogo from "./components/CodingFactoryLogo.tsx";

<>
  <CodingFactoryLogo />
</>
```

#### Layout.tsx
- εδώ έχω βάλει ένα `{childrean}` Που ως props θα περνάει τα διάφορα υπο components μου. **Στο App.tsx θα είναι μέσα σε `<Layout></Layout>`**
- τα header και footer δεν υπάρχουν ακόμα
- προσοχή εδώ είναι tsx όποτε χριάζετε να δηλώνω τύπο και interface η type
- `children: React.ReactNode` δεν είναι string boolean κλπ είναι ReactNode δηλ ένα εγγυρο αντικείμενο της react. θα μπορούσε να είναι και component
- απο το viewport θα πάρεις το 95% `min-h-[95vh]` πάμε το περιεχόμενο λίγο πιο κάτω `min-h-[95vh] pt-24`
```tsx
interface LayoutProps{
  children: React.ReactNode;
}
const Layout = ({children}:LayoutProps) => {}
```
```tsx
import React from "react";
import Header from "./Header.tsx";
import Footer from "./Footer.tsx";

interface LayoutProps{
  children: React.ReactNode;
}

const Layout = ({children}:LayoutProps) => {
  return (
    <>
      <Header/>
        <div className="container mx-auto min-h-[95vh] pt-24">
          {children}
        </div>
      <Footer/>
    </>
  )
}

export default Layout;
```
- App.tsx
```tsx
import Layout from "./components/Layout.tsx";

<>
  <Layout></Layout>
</>
```

#### Header.tsx
- Πάει την γραμμη του λινκ πιο κατω `hover:underline hover:underline-offset-4`
- `px-4` και δεξια και αριστερά
- αργότερα θα αλλάξουμε το `<a>` με router
```tsx
import CodingFactoryLogo from "./CodingFactoryLogo.tsx";

const Header = () => {
  return (
    <>
      <header className="bg-[#782024] fixed w-full">
        <div className="container mx-auto px-4 flex items-center justify-between">
          <CodingFactoryLogo />
          <a className="text-white hover:underline hover:underline-offset-4" href="/">Home</a>
        </div>
      </header>
    </>
  )
}
export default Header;
```

#### Footer.tsx
```tsx
const Footer = () => {
  const currentYear: number = new Date().getFullYear();

  return (
  <>
    <footer className="bg-gray-700">
      <div className="text-white text-center py-4">
        Copyright © {currentYear}, Coding Factory 7. All rights reserved.
      </div>
    </footer>
  </>
  )
}

export default Footer;
```

- App.tsx
```tsx
import CodingFactoryLogo from "./components/CodingFactoryLogo.tsx";
import Layout from "./components/Layout.tsx";

function App() {

  return (
    <>
      <Layout>
        <CodingFactoryLogo />
      </Layout>
    </>
  )
}

export default App
```

# State
#### ClassComponentWithState.tsx
- θα φτιαξουμε έναν counter
- χρειαζομαι τον τυπο της ts
```tsx
type State = {
  count: number;
}
```
- `<object, State>` το object είναι το Props μου και state o τύπος μου
- `onClick={this.increase}`
- πως θα ήταν αν ήθελα state σε δύο μεταβλητές;
```tsx
  constructor(props: object) {
    super(props);
    this.state = { count: 0 };
  }
```
 

```tsx
import { Component } from "react";

type State = {
  count: number;
}

class ClassComponentWithState extends Component<object, State> {
  // εδώ φτιαχνω το αρχικό μου state
  constructor(props: object) {
    super(props);
    this.state = { count: 0 };
  }

  increase = () => {
    this.setState({count: this.state.count + 1})
  }

  reset = () => {
    this.setState({count: 0})
  }

  render() {
    return(
    <>
      <div className="space-y-4 pt-12">
        <h1 className="text-center">Count is {this.state.count}</h1>
        <div className="text-center space-x-4">
        <button
          className="bg-black text-white py-2 px-4"
          onClick={this.increase}
        >
          Increase
        </button>
        <button
          className="bg-red-400 text-white py-2 px-4"
          onClick={this.reset}
        >
          Reset
        </button>
        </div>
      </div>
    </>
    )
  }
}

export default ClassComponentWithState;
```

- App.tsx
```tsx
import ClassComponentWithState from "./components/ClassComponentWithState.tsx";
    <>
      <Layout>
        <ClassComponentWithState/>
      </Layout>
    </>
```

#### FunctionalComponentWithState.tsx
- εδω η συνταξη του useState
```tsx
import { useState } from "react";
const [count,setCount] = useState(0);
```
- καλώ τις function χωρις (): `onClick={resetCount}`
```tsx
import { useState } from "react";

const FunctionalComponentWithState = ( ) => {
  const [count,setCount] = useState(0);

  const increaseCount = () => {
    setCount(count + 1)
  }

  const resetCount = () => {
    setCount(0)
  }

  return (
    <>
      <div className="space-y-4 pt-12">
        <h1 className="text-center">Count is {count}</h1>
        <div className="text-center space-x-4">
          <button
            className="bg-black text-white py-2 px-4"
            onClick={increaseCount}
          >
            Increase
          </button>
          <button
            className="bg-red-400 text-white py-2 px-4"
            onClick={resetCount}
          >
            Reset
          </button>
        </div>
      </div>
    </>
  )
}

export default FunctionalComponentWithState;
```

- App.tsx
```tsx
import FunctionalComponentWithState from "./components/FunctionalComponentWithState.tsx";
    <>
      <Layout>
        <FunctionalComponentWithState/>
      </Layout>
    </>
```
## μεταφορα state στα child components
- φτιαχνω λίγο καλήτερα τον μετρητή
#### Counter.tsx
- θα πρέπει να φτιαχτεί το CounterButton
- η σύνταξη για να περάσω Props στα child component `<CounterButton onClick={resetCount} disabled={count === 0} label="Reset" addClass="bg-cf-dark-red"/>`, αλλου ={}, αλλού ={x>==y}, αλλου =""
```tsx
import { useState } from "react";
import CounterButton from "./CounterButton.tsx";

const Counter = ( ) => {
  const [count,setCount] = useState(0);

  const increaseCount = () => {
    setCount(count + 1)
  }

  const decreaseCount = () => {
    if (count > 0) {
      setCount(count - 1)
    }
  }

  const resetCount = () => {
    setCount(0)
  }

  return (
    <>
      <div className="space-y-4 text-2xl pt-12">
        <h1 className="text-center">Count is {count}</h1>
        <div className="text-center space-x-4">
          <CounterButton onClick={increaseCount} label="Increase" />
          <CounterButton onClick={decreaseCount} disabled={count === 0} label="Decrease" />
          <CounterButton onClick={resetCount} disabled={count === 0} label="Reset" addClass="bg-cf-dark-red"/>
        </div>
      </div>
    </>
  )
}

export default Counter;
```

### διαφορα για μεταφορά σε child style/default if not etxc
#### CounterButton.tsx
- ως props έρχονται onClick, disabled = false, label, addClass. Μετά λογο ts πρέπει να βάλω `({ ... }: ButtonProps)`
- το addClass το ορίζω επιτόπου και δεν το φέρνω απο τον πατέρα. 
- το disabled στον πατέρα δεν είναι state αλλα μια boolean συνάρτηση `disabled={count === 0}`
- το OnClick είναι μια συνάρτηση. ορίζετε κάπως περίοεγα στο ts type `onClick: () => void;`
- επειδή στο type το disabled είναι optional ('disabled?: boolean;'), στην εισαγώγή των Props στην συναρτηση δίνω και την default τιμή που θα έχει άν δεν δώσει ο πατέρας 'disabled = false'
- ιδιο και με `addClass?: string;`, `addClass = "bg-cf-dark-gray"`
- προσοχή πως περνάω κλάσεις και αλλαγή τους απο τον πατέρα. πχ `className={"disabled:bg-gray-600 text-white py-2 px-4 " + addClass}` (προσοχή θέλει ένα ' ')

```tsx
type ButtonProps = {
  onClick: () => void;
  disabled?: boolean;
  label: string;
  addClass?: string;
}

const CounterButton = ({onClick, disabled = false, label, addClass = "bg-cf-dark-gray"}: ButtonProps) => {
  return (
    <>
      <button
        className={"disabled:bg-gray-600 text-white py-2 px-4 " + addClass}
        onClick={onClick}
        disabled={disabled}
      >
        {label}
      </button>
    </>
  )
}
export default CounterButton;
```
- App.tsx
```tsx
import Layout from "./components/Layout.tsx";
import Counter from "./components/Counter.tsx";
function App() {
  return (
    <>
      <Layout>
        <Counter/>
      </Layout>
    </>
  )
}
export default App
```

**εικονήδια lucid react** 
- 26/5/2025

- from gpt για update των λόκαλ αρχείων απο το τελικό version του μαθήματος:
git remote -v

git fetch teacher --tags
➡️ Fetches all tags and updates from the teacher remote.
This pulls any tags (like 2025.05.27) and refs without merging them into your working branch.

git checkout -b temp-2025.05.27 2025.05.27
➡️ Creates and checks out a new branch named temp-2025.05.27 from the tag 2025.05.27.
This allows you to work with the snapshot of code associated with that tag.

git checkout main
➡️ Switches back to your main branch.
You'll merge the tag into your main branch next.

git merge temp-2025.05.27 --allow-unrelated-histories
➡️ Merges the temp-2025.05.27 branch into main, even if they don't share a common history.
This is useful if the tag represents a separate repository or unrelated commit history (common in teacher repos or exercises).

git branch -d temp-2025.05.27
➡️ Deletes the temporary branch temp-2025.05.27.
This is safe after a successful merge.

git push origin main
➡️ Pushes your updated main branch to your remote GitHub (or other) repository.
This publishes your merged changes online.

#### NameChanger.tsx
- λογο ts: `(e: React.ChangeEvent<HTMLInputElement>)`
- `e.target.value`

```tsx
import { useState } from 'react';

const NameChanger = () => {
  const [name, setName] = useState("");

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setName(e.target.value);
  }

  return (
    <>
      <h1 className="text-center text-xl pt-4">Hello, {name || "Stranger"}</h1>
      <div className="text-center mt-4">
        <input
          type="text"
          value={name}
          onChange={handleChange}
          className="border px-4 py-2"
        />
      </div>
    </>
  )
}
export default NameChanger;
```

- App.tsx
```tsx
import Layout from "./components/Layout.tsx";
import NameChanger from "./components/NameChanger.tsx";
function App() {
  return (
    <>
      <Layout>
        <NameChanger/>
      </Layout>
    </>
  )
}
export default App
```

#### CounterWithMoreStates.tsx

```tsx
import {useState} from "react";
import CounterButton from "./CounterButton.tsx";

const CounterWithMoreStates = () =>{
  const [count,setCount] = useState(0);
  const [lastAction, setLastAction] = useState("");
  const [time, setTime] = useState("");

  const getCurrentTime = () => new Date().toLocaleTimeString();

  const increaseCount = () => {
    setCount(count + 1);
    setLastAction("Increase");
    setTime(getCurrentTime());
  }

  const decreaseCount = () => {
    if (count > 0) {
      setCount(count - 1);
      setLastAction("Decrease");
      setTime(getCurrentTime());
    }
  }

  const resetCount = () => {
    setCount(0);
    setLastAction("Reset");
    setTime(getCurrentTime());
  }

  return (
    <>
      <div className="space-y-4 text-2xl pt-12">
        <h1 className="text-center">Count is {count}</h1>
        <div className="text-center space-x-4">
          <CounterButton onClick={increaseCount} label="Increase" />
          <CounterButton onClick={decreaseCount} disabled={count === 0} label="Decrease" />
          <CounterButton onClick={resetCount} disabled={count === 0} label="Reset" addClass="bg-cf-dark-red"/>
        </div>
      </div>
      <p className="text-center pt-8">Last change: <strong>{lastAction || "-"}</strong> at <strong>{time || "-"}</strong></p>
    </>
  )
}
export default CounterWithMoreStates;
```

- App.tsx
```tsx
import Layout from "./components/Layout.tsx";
import CounterWithMoreStates from "./components/CounterWithMoreStates.tsx";
function App() {
  return (
    <>
      <Layout>
        <CounterWithMoreStates/>
      </Layout>
    </>
  )
}
export default App
```

- δεν είναι καλή πρακτική να έχουμε state χωριστά που να ενημερώνονται πάντα ταυτόχρονα και θα πρέπει να τα ομαδοποιήσουμε κάπως.
### χρήση ομαδοποιημένου state
#### CounterAdvanced.tsx
- ομαδοποίηση:
```tsx
  const [state, setState] = useState<CounterState>({
    count:0,
    lastAction: "",
    time: "",
  });
  //...
  setState({
    count: state.count + 1,
    lastAction: "Increase",
    time: getCurrentTime(),
  });
```
- στην Html καλούνται ως `{state.lastAction || "-"}` κλπ
- χρειάζομαι ts types για αυτό `useState<CounterState>`:
- < > → Για generics (συναρτήσεις/components που δουλεύουν με πολλούς τύπους).useState<number>, Array<string>, Promise<boolean>.  
: → Για αναφορά τύπων σε μεταβλητές, κλάσεις, functions.
const name: string, function greet(): void.

```tsx
type CounterState ={
  count: number;
  lastAction: string;
  time: string;
}
```

```tsx
import {useState} from 'react';
import CounterButton from "./CounterButton.tsx";

type CounterState ={
  count: number;
  lastAction: string;
  time: string;
}

const CounterAdvanced = () => {

  const [state, setState] = useState<CounterState>({
    count:0,
    lastAction: "",
    time: "",
  });

  const getCurrentTime = () => new Date().toLocaleTimeString();

  const increaseCount = () =>{
    setState({
      count: state.count + 1,
      lastAction: "Increase",
      time: getCurrentTime(),
    });
  }

  const decreaseCount = () =>{
    if (state.count > 0){
      setState({
        count: state.count -1,
        lastAction: "Decrease",
        time: getCurrentTime(),
      });
    }
  }

  const resetCount = () =>{
    setState({
      count: 0,
      lastAction: "Reset",
      time: getCurrentTime(),
    });
  }

  return (
    <>
      <div className="space-y-4 text-2xl pt-12">
        <h1 className="text-center">Count is {state.count}</h1>
        <div className="text-center space-x-4">
          <CounterButton onClick={increaseCount} label="Increase" />
          <CounterButton onClick={decreaseCount} disabled={state.count === 0} label="Decrease" />
          <CounterButton onClick={resetCount} disabled={state.count === 0} label="Reset" addClass="bg-cf-dark-red"/>
        </div>
      </div>
      <p className="text-center pt-8">Last change: <strong>{state.lastAction || "-"}</strong> at <strong>{state.time || "-"}</strong></p>
    </>
  )
}
export default CounterAdvanced;
```

- App.tsx
```tsx
import Layout from "./components/Layout.tsx";
import CounterAdvanced from "./components/CounterAdvanced.tsx";
function App() {
  return (
    <>
      <Layout>
        <CounterAdvanced/>
      </Layout>
    </>
  )
}
export default App
```

# Custom hook
- αλλα Hooks useEffect, userRef, useReduce, useStateSction
- αλλα μπορώ να φτιάξω και custom για επαναχρησιμοποίση κωδικα

#### useCounter.ts
- εδώ θα βγάλω όλη την λειτορυγία σε ένα custom hook και θα κρατήσω στο αρχείο μου μόνο το rendering
- τα βάζω στον φάκελο hooks. Συνηθίζετε να τα ονομαζω με το προθεμα use. Και είναι αρχείο ts (οχι tsx). και δεν εχουμε επιστροφή jsx/tsx
- `export const` και `return {count, increase, decrease, reset};`

```ts
import { useState } from "react";

export const useCounter = () => {
  const [count, setCount] = useState(0);

  const increase = () => {
    setCount(count + 1);
  }
  const decrease = () => {
    if (count > 0){
      setCount(count - 1);
    }
  }
  const reset = () => {
    setCount(0);
  }
  return {
    count,
    increase,
    decrease,
    reset
  };
}
```
#### CounterWithCustomHook.tsx
- παίρνω την λογική απο το custom hook `const { count, increase, decrease, reset } = useCounter();`
- state.count -> count etc.
```tsx
import CounterButton from "./CounterButton.tsx";
import { useCounter } from "../hooks/useCounter.ts"

const CounterWithCustomHook = () => {

  // custom hook function
  const { count, increase, decrease, reset } = useCounter();

  return (
    <>
      <div className="space-y-4 text-2xl pt-12">
        <h1 className="text-center">Count is {count}</h1>
        <div className="text-center space-x-4">
          <CounterButton onClick={increase} label="Increase" />
          <CounterButton onClick={decrease} disabled={count === 0} label="Decrease" />
          <CounterButton onClick={reset} disabled={count === 0} label="Reset" addClass="bg-cf-dark-red"/>
        </div>
      </div>
      {/*<p className="text-center pt-8">Last change: <strong>{state.lastAction || "-"}</strong> at <strong>{state.time || "-"}</strong></p>*/}
    </>
  )
}

export default CounterWithCustomHook;
```

- App.tsx
```tsx
import Layout from "./components/Layout.tsx";
import CounterWithCustomHook from "./components/CounterWithCustomHook.tsx";
function App() {

  return (
    <>
      <Layout>
        <CounterWithCustomHook/>
      </Layout>
    </>
  )
}
export default App
```

#### useAdvancedCounter.ts
- περνάω εξω και το state. γιατί πιά είναι έδώ
```ts
  return {
    count: state.count,
    lastAction: state.lastAction,
    time: state.time,
    increase,
    decrease,
    reset
  };
```
```ts
import {useState} from 'react';

type CounterState ={
  count: number;
  lastAction: string;
  time: string;
}

export const useAdvancedCounter = () => {

  const [state, setState] = useState<CounterState>({
    count:0,
    lastAction: "",
    time: "",
  });

  const getCurrentTime = () => new Date().toLocaleTimeString();

  const increase =() => {
    setState({
      count: state.count + 1,
      lastAction: "Increase",
      time: getCurrentTime(),
    });
  };

  const decrease = () => {
    if (state.count > 0){
      setState({
        count: state.count - 1,
        lastAction: "Decrease",
        time: getCurrentTime(),
      });
    }
  };

  const reset = () => {
    setState({
      count: 0,
      lastAction: "Reset",
      time: getCurrentTime(),
    })
  }

  return {
    count: state.count,
    lastAction: state.lastAction,
    time: state.time,
    increase,
    decrease,
    reset
  };
}
```

#### CounterAdvancedWithCustomHook.tsx

```tsx
import CounterButton from "./CounterButton.tsx";
import { useAdvancedCounter } from "../hooks/useAdvancedCounter.ts";

const CounterAdvancedWithCustomHook = () => {

  // custom hook function
  const { count, lastAction, time, increase, decrease, reset } = useAdvancedCounter();

  return (
    <>
      <div className="space-y-4 text-2xl pt-12">
        <h1 className="text-center">Count is {count}</h1>
        <div className="text-center space-x-4">
          <CounterButton onClick={increase} label="Increase" />
          <CounterButton onClick={decrease} disabled={count === 0} label="Decrease" />
          <CounterButton onClick={reset} disabled={count === 0} label="Reset" addClass="bg-cf-dark-red"/>
        </div>
      </div>
      <p className="text-center pt-8">Last change: <strong>{lastAction || "-"}</strong> at <strong>{time || "-"}</strong></p>
    </>
  )
}

export default CounterAdvancedWithCustomHook;
```

- App.tsx
```tsx
import Layout from "./components/Layout.tsx";
import CounterAdvancedWithCustomHook from "./components/CounterAdvancedWithCustomHook";
function App() {
  return (
    <>
      <Layout>
        <CounterAdvancedWithCustomHook/>
      </Layout>
    </>
  )
}
export default App
```
- **27/5/2025**
# σύνθετα states και useReducer
## const [state, dispatch] = useReducer(reducer, initialArg, init?)
- παίρνει αρχικό state και ένα action 

- πρώτα φτιάχνω το custom hook για τον reducer και μετά τον περνάω στο tsx και τέλος στο App για να προβληθεί
#### useCounterWithReducer.ts
- 1-> δηλώνω τύπους 2-> φτιάχνω action 3-> φτιάχνω το initial State 4-> φτιάχνω τον reducer(state,action) 5-> φτιάχνω τον useReducer (reducer, initialState)
- o reducer παίρνει (state,action) ενώ ο useReducer παίρνει (reducer, initialState)

```ts
// το είδος της δράσης που θα περάσω μέσα στον reducer (σαν redux)
// o reducer θέλει αρχικο state και δράση
type Action =
  | {type: "INCREASE"}
  | {type: "DECREASE"}
  | {type: "RESET"};
```


```ts
import { useReducer } from 'react';

// πριν απο την κεντρική μου συνάρτηση όρισα διάφορα function types. η εκεντρική είναι αυτήπου κάνω export

// 1. ξεκινάω με τα types
type CounterState ={
  count: number;
  lastAction: string;
  time: string;
}

// 2. το είδος της δράσης που θα περάσω μέσα στον reducer (σαν redux)
// o reducer θέλει αρχικο state και δράση. Η δράση αυτή θα εφαρμοστεί στην συνάρτηση του reducer
// The '|' symbol in TypeScript is called a union type operator, and it means "OR"
type Action =
  | {type: "INCREASE"}
  | {type: "DECREASE"}
  | {type: "RESET"};

// 3. φτιάχνω την αρχική μου state
const initialState: CounterState = {
  count: 0,
  lastAction: "",
  time: "",
}

const getCurrentTime = () => new Date().toLocaleTimeString();

// 4. o reducer παίρνει (state,action) ενώ ο useReducer παίρνει (reducer, initialState)
function reducer(state:CounterState, action:Action): CounterState {
  // περιμένουμε να λάβουμε το action.type και κάνουμε switch για τις τρείς περιπτώσεις
 switch (action.type) {
   case "INCREASE":
     return {
       count: state.count + 1,
       lastAction: "Increase",
       time: getCurrentTime(),
     };
     // εδώ έχει έναν turnary oparator
   case "DECREASE":
     return state.count > 0
      ? {
         count: state.count - 1,
         lastAction: "Decrease",
         time: getCurrentTime(),
       }
       : state;
   case "RESET":
     return {
       count: 0,
       lastAction: "Reset",
       time: getCurrentTime(),
     };
    // αν δεν γίνει καποια αλλαγή το επιστρέφει οπως είναι
   default:
     return state;
 }
}

// 5.
export const useCounterWithReducer = () => {

  const [state, dispatch] = useReducer(reducer, initialState);

  // το dispatch έιναι μέρος της συνταξης του useReducer. 
  // εδώ φτιαχνω 3 συναρτίσεις που αντιστοιχούν στις δράσεις μου. λένε "όταν με καλέίς στέλνω/dispatch μια "λέξη" στο switch"
  const increase = () => dispatch({type: "INCREASE"});
  const decrease = () => dispatch({type: "DECREASE"});
  const reset = () => dispatch({type: "RESET"});

// επιστρέφω τις λειτουργίες μου και το state που πια βρίσκετε εδώ και οχι στο component
  return {
    count: state.count,
    lastAction: state.lastAction,
    time: state.time,
    increase,
    decrease,
    reset,
  };
};
```

#### CounterWithReducer.tsx
- καλώ το hook που έχει όλη την λογική `const {count, lastAction, time, increase, decrease, reset} = useCounterWithReducer();`
```tsx
import CounterButton from "./CounterButton.tsx";
import { useCounterWithReducer } from "../hooks/useCounterWithReducer.ts";

const CounterWithReducer = () => {

  const {count, lastAction, time, increase, decrease, reset} = useCounterWithReducer();

  return (
    <>
      <div className="space-y-4 text-2xl pt-12">
        <h1 className="text-center">Count is {count}</h1>
        <div className="text-center space-x-4">
          <CounterButton onClick={increase} label="Increase" />
          <CounterButton onClick={decrease} disabled={count === 0} label="Decrease" />
          <CounterButton onClick={reset} disabled={count === 0} label="Reset" addClass="bg-cf-dark-red"/>
        </div>
      </div>
      <p className="text-center pt-8">Last change: <strong>{lastAction || "-"}</strong> at <strong>{time || "-"}</strong></p>
    </>
  )
}

export default CounterWithReducer;
```

- App.tsx
```tsx
import Layout from "./components/Layout.tsx";
import CounterWithReducer from "./components/CounterWithReducer.tsx";
function App() {
  return (
    <>
      <Layout>
        <CounterWithReducer/>
      </Layout>
    </>
  )
}
export default App
```

# todo
- μικρό πρότζεκτ todo με τρεία components και useReducer
- φακελος todo μέσα στα components
- εικονίδια: `npm install lucide-react`

#### Todo.tsx
- παρακάτω φτιαχνω ως χωριστά components τα TodoForm TodoList
- η useReducer είναι μονο στο μητρικό component και στα child περνάω μονο την dispatch. παρολα αυτά το type πρέπει να δηλωθέι και στο παιδί. (το σωστό θα ήταν να είχα κάπου δηλωμένα τα types και να τα καλώ απο εκεί. ToDo αργότερα)
```tsx
import { useReducer } from 'react';
import TodoForm from "./TodoForm.tsx";
import TodoList from "./TodoList.tsx";


// Πρώτα δηλώνω τους τύπους μου για την ts
type TodoProps = {
  id: number;
  text:string;
}

// φτιάχνω το action του reducer 
type Action =
  | {type: "ADD"; payload: string}
  | {type: "DELETE"; payload: number}

// φτιάχνω τον reducer με το switch για τα διάφορα action
// επιστρέφει ': TodoProps[]'
const todoReducer = (state: TodoProps[], action: Action): TodoProps[] => {
  switch (action.type) {
    // το todo είναι array απο objects. Πρέπει να φτιάξω ένα νέο obj και να το προσθέσω στο arr
    case "ADD":{
      const newTodo: TodoProps = {
        id: Date.now(),
        text: action.payload,
      };
      // spread oparator
      return [...state, newTodo];
    }
    case "DELETE":
      // το delete γινετε με filter oπου κράτάει μονο όποιο δεν έχει το id Που ψάχνω
      return state.filter(todo => todo.id !== action.payload);
    default:
      return state;
  }
};

const Todo = () =>{
  const [todos, dispatch] = useReducer(todoReducer, []);

  return (
    <>
      <div className="max-w-sm mx-auto p-6">
        <h1 className="text-center text-2xl mb-4">To-Do List</h1>
        // 
        <TodoForm dispatch={dispatch} />
        <TodoList todos={todos} dispatch={dispatch} />
      </div>
    </>
  )
};

export default Todo;
```

#### TodoForm.tsx
- `dispatch: React.Dispatch<Action>`
- `(e: React.ChangeEvent<HTMLInputElement>)` και `e.target.value`
- οπως επίσης `(e: React.FormEvent)`
- στο onSubmit συμμαντικό `dispatch({type: "ADD", payload: text});`

```tsx
import { useState } from "react";

// δεν κατάλαβα γιατί δηλώνουμε ξανα το action του reducer και δεν είναι στο Parent αρχειο
type Action =
  | {type: "ADD"; payload: string}
  | {type: "DELETE"; payload: number}


type TodoFormProps = {
  dispatch: React.Dispatch<Action>;
};

// το μόνο Prop που έχει είναι το dispatch το οποίο μάλιστα ορίζετε στο ίδιο component
const TodoForm = ({ dispatch }: TodoFormProps) => {

  const [text, setText] = useState("");

  // φτιάχνω ένα state και μια handleChange για να ακολουθεί τη δακτυλογράφηση το τι προβάλει η οθόνη
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setText(e.target.value);
  };

  // για το κουμπί add
  const handleSubmit = (e: React.FormEvent) =>{
    e.preventDefault();
    // μην το περάσεις αν το μυνημα είναι μόνο spaces
    if (text.trim() !== "") {
      dispatch({type: "ADD", payload: text});
      setText("");
    }
  };

  return (
    <>
      <form
        className="flex gap-4 mb-4"
        onSubmit={handleSubmit}
      >
        <input
          type="text"
          value={text}
          onChange={handleChange}
          className="flex-1 border p-2 rounded"
          placeholder="New task.."
        />
        <button
          type="submit"
          className="bg-cf-dark-gray text-white px-4 py-2 rounded"
        >
          Add
        </button>
      </form>
    </>
  )
};

export default TodoForm;
```

#### TodoList.tsx
```tsx
import { Trash2 } from "lucide-react";

type Todo = {
  id: number;
  text: string;
}

type TodoListProps = {
  todos: Todo[];
  dispatch: React.Dispatch<{type: "DELETE"; payload: number}>
}

const TodoList = ({todos, dispatch}: TodoListProps) =>{

// εδω όμως δεν ορισαμε το dispatch type Οπως κάναμε παραπάνω στο TodoFormProps
//gpt:
/*
 Πράγματι, στο TodoList.tsx έχεις περάσει το type του dispatch "επί τόπου":
dispatch: React.Dispatch<{type: "DELETE"; payload: number}>
Ενώ στο TodoForm.tsx το έκανες πιο σωστά, ορίζοντας ένα κοινό τύπο Action και τον χρησιμοποιείς έτσι:
dispatch: React.Dispatch<Action>
*/
const handleDelete = (id: number) => () => {
  dispatch({type: "DELETE", payload: id});
}

  return (
    <>
      <ul className="space-y-2">
        // το todo ήρθε απ'έξω
        // τα map θέλουν υποχρεωτικά key
        {todos.map(todo => (
          <li key={todo.id} className="flex items-center justify-between bg-gray-100 p-2 rounded">
            <span>{todo.text}</span>
            <button
              onClick={handleDelete(todo.id)}
              className="text-cf-dark-red"
            >
              // εικονήδιο οπως το έκκανα import απο την βιβλιοθήκη της Lucid-react
              <Trash2 size={18}/>
            </button>
          </li>
          ))}
      </ul>
    </>
  )
}

export default TodoList;
```

- App.tsx
```tsx
import Layout from "./components/Layout.tsx";
import Todo from "./components/Todo/Todo.tsx";

function App() {

  return (
    <>
      <Layout>
        <Todo/>
      </Layout>
    </>
  )
}
export default App
```

- **28/5/2025**
# todo (ανεξάρτητο)
- pulling git daily version from lesson:
```bash
# Fetch tags and branches from the teacher’s remote
git fetch teacher --tags
# Create a new branch from the fetched tag
git checkout -b temp-2025.05.28 2025.05.28
# Go back to your main branch
git checkout main
# Merge the contents from the tag-based branch into your main branch
# resolve conflicts by opening the files
git merge temp-2025.05.28
# Optional: delete the temp branch if merge is successful
git branch -d temp-2025.05.28
```

στην περιπτωσή αλλάζουμε το state με setState και στην άλλη με dispatch
- συνέχεια στο Todo app
o καθηγητής έφτιαξε ένα νεο repo για το todo app

```bash
git init
git remote add origin git@github.com:alkisax/cf7-react-todo-app.git
git pull origin main --allow-unrelated-histories
git add .
git commit -m "initial push"
git push origin main
git remote add teacher https://github.com/DamMarin/cf7-react-todo-app.git
git fetch teacher
git merge teacher/main --allow-unrelated-histories
# (Resolve merge conflicts if any, e.g. in .gitignore)
git add .gitignore  # after conflict resolved
git commit -m "Merge teacher/main with resolved conflict"

npm install
```

## αρχικές ρυθμήσεις
```bash
npm create cite@latest . -- --template react-ts
npm install
npm install tailwindcc @tailwindcss/vite lucide-react
```

#### index.css
```css
@import "tailwindcss";

@theme {
  --color-cf-dark-red: #782024;
  --color-cf-dark-gray: #202123;
  --color-cf-gray: #2b2d2f;
}
```

#### vite.config.ts
```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

// https://vite.dev/config/
export default defineConfig({
  plugins: [react(),tailwindcss()],
})
```

- αντιγράφω τα components απο το προηγούμενο project
- App.tsx
```tsx
import Todo from "./components/Todo.tsx";
function App() {
  return (
    <>
      <Todo/>
    </>
  )
}
export default App
```
## μεταφορά όλων των types σε ένα σημείο για να τα καλώ 
- μετα θα σβήσω τα types απο τα components και θα τα καλώ
#### types.ts
```ts
export type TodoProps = {
  id: number;
  text: string;
  completed: boolean;
}

export type Action =
  | {type: "ADD"; payload: string}
  | {type: "DELETE"; payload: number}
  | {type: "EDIT"; payload: {id: number; newText:string} }
  | {type: "COMPLETE"; payload: number};

export type TodoFormProps = {
  dispatch: React.Dispatch<Action>;
};

export type TodoListProps = {
  todos: TodoProps[];
  dispatch: React.Dispatch<Action>
}

```
και
- Todo.tsx
```tsx
import type { TodoProps, Action} from "../types.ts"
```
- TodoForm.tsx
```tsx
import type { TodoFormProps } from "../types.ts";
```
- TodoList.tsx
```tsx
import type { TodoListProps } from "../types.ts";
```


## eddit feat
#### types.ts
```ts
export type Action =
//etc
  // Χρειάζομαι το id του todo που θα αλλάξω και το νέο κείμενο που θα μπει
  | {type: "EDIT"; payload: {id: number; newText:string} }
//etc
```

#### TodoList.tsx
- δεν το κατάλαβα καλα αυτό `useState<number | null>(null)`
```tsx
import { useState } from "react";
import {Trash2, Edit, Save, X, Square, CheckSquare} from "lucide-react";
import type { TodoListProps } from "../types.ts";

const TodoList = ({todos, dispatch}: TodoListProps) =>{
const [editId, setEditId] = useState<number | null>(null);
const [editText, setEditText] = useState("");


// Version 1: () => () => { ... } You use this when you need to delay execution until something happens, like a button click.
// Version 2: () => { ... } This runs immediately when called.
// θα κάνουμε useState αντι για dispatch. αφού κάνουμε την αλλαγή θα κάνουμε το dispatch Μέσο handleSave
const handleEdit = (id: number, text: string) => () => {
  setEditId(id);
  setEditText(text);
}

const handleSave = (id: number) => () =>{
  dispatch({type: "EDIT", payload: {id, newText: editText}});
  setEditId(null);
  setEditText("");
}

const handleCancel = () => {
  setEditId(null);
  setEditText("");
}

const handleDelete = (id: number) => () => {
  dispatch({type: "DELETE", payload: id});
}

const handleToggle = (id: number) => () => {
  dispatch({type: "COMPLETE", payload: id});
}

            // εχω ένα state και ελέγχω αν το id του todo έχει διαλεχθει για αλλαγή τότε να μου εμφανήσει μια φόρμα για να προσθέσω το νέο κείμενο. Οποτε μου ανοίγει ένα input και ένα κουμπί save/cancel.
            // αλλιώς μου διχνει το todo με τις διαφορες επιλογές του
            // (e) => setEditText(e.target.value) για να φορτώνει την πληκτρολογιση
  return (
    <>
      <ul className="space-y-2">
        {todos.map(todo => (
          <li key={todo.id}
            className={`flex items-center justify-between bg-gray-100 p-2 rounded
             ${todo.completed ? "opacity-60 line-through" : ""}`}
          >
            { editId === todo.id ? (
              <>
                <div className="flex flex-1 gap-2">
                  <input
                    type="text"
                    value={editText}
                    onChange={(e) => setEditText(e.target.value)}
                    className="flex-1 border rounded p-1"
                  />
                  <button
                    onClick={handleSave(todo.id)}
                    className="text-cf-gray"
                  >
                    <Save size={18}/>
                  </button>
                  <button
                    onClick={handleCancel}
                    className="text-cf-dark-red"
                  >
                    <X size={18}/>
                  </button>
                </div>
              </>
            ) : (
              <>
                <div className="flex items-center gap-2 flex-1">
                  <button
                    className="text-green-500"
                    onClick={handleToggle(todo.id)}
                  >
                    {todo.completed ? (
                      <CheckSquare size={18}/>
                    ): (
                      <Square size={18}/>
                    )}
                  </button>
                  <span>{todo.text}</span>
                </div>
                <div className="flex gap-2">
                  <button
                    onClick={handleEdit(todo.id, todo.text)}
                    className="text-cf-gray"
                  >
                    <Edit size={18}/>
                  </button>
                  <button
                    onClick={handleDelete(todo.id)}
                    className="text-cf-dark-red"
                  >
                    <Trash2 size={18}/>
                  </button>
                </div>
              </>
            )}
          </li>
          ))}
      </ul>
    </>
  )
}

export default TodoList;
```
- 1:12:50 28/5/2025

- ξανα μόνο η λογική του edit και οχι τα της εμφάνησης
```tsx
const [editId, setEditId] = useState<number | null>(null);
const [editText, setEditText] = useState("");

const handleEdit = (id: number, text: string) => () => {
  setEditId(id);
  setEditText(text);
}

const handleSave = (id: number) => () =>{
  dispatch({type: "EDIT", payload: {id, newText: editText}});
  setEditId(null);
  setEditText("");
}
            // το editId μου έρχετε απο το κουμπί edit που καλέι το handleEdit που αλλάζει το state
            { editId === todo.id ? (
                  <input
                    type="text"
                    value={editText}
                    onChange={(e) => setEditText(e.target.value)}
                    className="flex-1 border rounded p-1"
                  />
                  
                  <button
                    onClick={handleSave(todo.id)}
                    className="text-cf-gray"
                  >
                    <Save size={18}/>
                  </button>

                  <button
                    onClick={handleCancel}
                    className="text-cf-dark-red"
                  >
                    <X size={18}/>
                  </button>

                ) : (

                  <button
                    onClick={handleEdit(todo.id, todo.text)}
                    className="text-cf-gray"
                  >
                    <Edit size={18}/>
                  </button>

                )
```
- delete
```tsx
const handleDelete = (id: number) => () => {
  dispatch({type: "DELETE", payload: id});
}

                  <button
                    onClick={handleDelete(todo.id)}
                    className="text-cf-dark-red"
                  >
```

- todo.tsx
προστέθηκαν τα edit complete στον switch
```tsx
const todoReducer = (state: TodoProps[], action: Action): TodoProps[] => {
  switch (action.type) {
// ...
    case "DELETE":
      return state.filter(todo => todo.id !== action.payload);
    case "EDIT":
      // αν υπάρχει id αλλάζει μόνο αυτό με το id. τα δεδομένα του έρχονται με το payload του action (dispatch({type: "EDIT", payload: {id, newText: editText}});). Αν δεν υπάρχει id (λάθος;) το αφήνει όπως είναι
      return state.map( todo =>
        todo.id === action.payload.id
        ? {...todo, text: action.payload.newText}
        : todo
      );
// ...
  }
```

## todo task done feat
#### TodoList.tsx
```tsx
const handleToggle = (id: number) => () => {
  dispatch({type: "COMPLETE", payload: id});
}


          <li key={todo.id}
            className={`flex items-center justify-between bg-gray-100 p-2 rounded
             ${todo.completed ? "opacity-60 line-through" : ""}`}
          >

                  <button
                    className="text-green-500"
                    onClick={handleToggle(todo.id)}
                  >
                    {todo.completed ? (
                      <CheckSquare size={18}/>
                    ): (
                      <Square size={18}/>
                    )}
                  </button>
```
- Todo.tsx
```tsx
    case "COMPLETE":
      return state.map( todo =>
        todo.id === action.payload
        ? {...todo, completed: !todo.completed}
        : todo
      );
```

## σημείωση useEffect
useEffect(setup, dependencies?)  
το setup είναι κάποιες function οι οποίες τρέχουν όταν ικανοποιηθεί το dependecy το οποίο είναι arr []  
πχ τρέχει όταν αλλάξει το state   

```tsx
const [state, setState] = useState("")  
const state = () =>  {console.log("hello") }  
useEffect(setup, [state])  
```
αν [] τρέχει οταν πρωτοφορτώσει η σελίδα  

#### NameChanger.tsx (απο το προηγούμενο project)
```tsx
import { useState, useEffect } from 'react';

const NameChanger = () => {
  const [name, setName] = useState("");

// θα τρέξει στην αλλαγή του state του name
  useEffect(() => {
    document.title = name ? `Hello, ${name}!` : "Hello, Stranger!";
  },[name])

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setName(e.target.value);
  }

  return (
    <>
      <h1 className="text-center text-xl pt-4">Hello, {name || "Stranger"}</h1>
      <div className="text-center mt-4">
        <input
          type="text"
          value={name}
          onChange={handleChange}
          className="border px-4 py-2"
        />
      </div>
    </>
  )
}
export default NameChanger;
```

-2/6/2025
##### 1. Fetch all updates and tags from teacher repo
git fetch teacher --tags
##### 2. Create a temporary branch from the tag 2025.06.02
git checkout -b temp-2025.06.02 2025.06.02
##### 3. Switch back to your main branch
git checkout main
##### 4. Merge the temp branch into main (handle conflicts if needed)
git merge temp-2025.06.02 --allow-unrelated-histories
##### 5. Delete the temp branch (optional, after successful merge)
git branch -d temp-2025.06.02
##### 6. Push changes to your personal remote GitHub repo (optional)
git push origin main
- τα ιδια πανω κατω και για το todo app και για το intro app

# useEffect

#### πχ...
```tsx
  useEffect(() => {
    document.title = name ? `Hello, ${name}!` : "Hello, Stranger!";
  },[name])

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setName(e.target.value);
  }
```
```tsx
  useEffect(() => {
    // setup
    return () => {
      // clean up
      // το return θα τρέξει οταν αλλάξει κάποιο dependency (πχ να θέλουμε να μηδενίσουμε μια μεταβλητη)
      // (runs before the effect re-runs or on unmount)
    }
  },[name])

```
```tsx
  useEffect(() => {
    const id: number = setInterval(() => console.log("tick"), timeout 1000)
    return () => clearInterval(id);
  },[])
```
-εχουμε επιστρέψει στο project cf7-react-intro
#### OnlineStatus.tsx

```tsx
import { useState, useEffect  } from "react";
const OnlineStatus = () => {
  // μας επιστρέφει αν μια συσκευή είναι συνδεδεμένη. boolean
  const [isOnline, setIsOnline] = useState(navigator.onLine);

  useEffect(() => {
    // ενημερώνει το state με τον setter
    const handler = () => setIsOnline(navigator.onLine);
    // επειδή σε διάφορους browser δεν δουλευει προσθέσαμε μια λειτουργία που κοιτά αν είναι συνδεδεμένος ο χρήστης αν πέντε δευτερολεπα (καλεί τον handler)
    const pollingId: number = setInterval(handler, 5000);

    // οταν γίνει Online/offline τρέξε την handler
    window.addEventListener("online", handler);
    window.addEventListener("offline", handler);

    // απο εδώ και πέρα μια clean up όπου αφαιρώ οτι είχα προσθέσει
    return () => {
      clearInterval(pollingId);
      window.removeEventListener("online", handler);
      window.removeEventListener("offline", handler);
    };
  }, []) // είναι κενο. θα τρέξει μόνο στο refresh της σελίδας

  return (
    <>
      <div className={`text-white text-center mt-12 mx-4 p-4 rounded ${ isOnline ? 
        "bg-green-500" : "bg-cf-dark-red"}`}>
        { isOnline ? "You are online!" : "You are offline!" }
      </div>
    </>
  )
}

export default OnlineStatus;
```

- App.tsx
```tsx
import Layout from "./components/Layout.tsx";
import OnlineStatus from "./components/OnlineStatus.tsx";

function App() {
  return (
    <>
      <Layout>
        <OnlineStatus/>
      </Layout>
    </>
  )
}
export default App
```